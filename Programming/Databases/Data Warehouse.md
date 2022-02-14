### Access patterns

![[DB_Access_Patterns.png]]

 At first, the same databases were used for both transaction processing and analytic queries. SQL turned out to be quite flexible in this regard: it works well for OLTP-type queries as well as OLAP-type queries.
 
There was a trend for companies to stop using their OLTP systems for analytics purposes, and to run the analytics on a separate database instead. This separate database was called a data warehouse.

 OLTP systems are usually expected to be highly available and to process trans‐ actions with low latency, since they are often critical to the operation of the business.
 
 ---
 
 ### Data Warehouse
 
 A *data warehouse*, by contrast, is a separate database that analysts can query to their hearts’ content, without affecting OLTP operations. The data warehouse contains a read-only copy of the data in all the various OLTP systems in the company. Data is extracted from OLTP databases (using either a periodic data dump or a continuous stream of updates), transformed into an analysis-friendly schema, cleaned up, and then loaded into the data warehouse. This process of getting data into the warehouse is known as *Extract–Transform–Load (ETL)* - see this approach in general below
 ![[DB_Warehouse_ETL.png]]
A big advantage of using a separate data warehouse, rather than querying OLTP systems directly for analytics, is that the data warehouse can be optimized for analytic access patterns.
Many database vendors now focus on supporting either transaction processing or analytics workloads, but not both.

---

### Star schema

*Star schema* (also known as *dimensional modeling*)
![[DB_Warehouse_Star_Schema.png]]
 Some of the columns in the fact table are attributes, such as the price at which the product was sold and the cost of buying it from the supplier. Other columns in the fact table are foreign key references to other tables, called dimension tables. As each row in the fact table represents an event, the dimensions represent the who, what, where, when, how, and why of the event. Even date and time are often represented using dimension tables, because this allows additional information about dates (such as public holidays) to be encoded, allowing queries to differentiate between sales on holidays and non-holidays.  

The name “star schema” comes from the fact that when the table relationships are visualized, the fact table is in the middle, surrounded by its dimension tables; the connections to these tables are like the rays of a star.

A variation of this template is known as the snowflake schema, where dimensions are further broken down into subdimensions

---

### Column-Oriented storage

Although fact tables are often over 100 columns wide, a typical data warehouse query only accesses 4 or 5 of them at one time.

In most OLTP databases, storage is laid out in a row-oriented fashion: all the values from one row of a table are stored next to each other. Document databases are simi‐ lar: an entire document is typically stored as one contiguous sequence of bytes.

The idea behind column-oriented storage is simple: don’t store all the values from one row together, but store all the values from each column together instead. If each column is stored in a separate file, a query only needs to read and parse those columns that are used in that query, which can save a lot of work. *Column storage is easiest to understand in a relational data model, but it applies equally to nonrelational data.*
![[DB_Warehouse_Column_oriented.png]]

Besides only loading those columns from disk that are required for a query, we can further reduce the demands on disk throughput by compressing data. Depending on the data in the column, different compression techniques can be used. One technique that is particularly effective in data warehouses is [[Bitmap]] encoding .

> *Cassandra* and *HBase* have a concept of column families, which they inherited from Bigtable. However, it is very misleading to call them column-oriented: within each column family, they store all columns from a row together, along with a row key, and they do not use column compression. Thus, the Bigtable model is still mostly row-oriented.

 

Besides reducing the volume of data that needs to be loaded from disk, column- oriented storage layouts are also good for making efficient use of CPU cycles. For example, the query engine can take a chunk of compressed column data that fits comfortably in the CPU’s L1 cache and iterate through it in a tight loop. Column compression allows more rows from a column to fit in the same amount of L1 cache. Operators, such as the bitwise AND and OR described previously, can be designed to operate on such chunks of compressed column data directly. This technique is known as *vectorized processing*

##### Sorting column-oriented storage

In a column store, it doesn’t necessarily matter in which order the rows are stored. It’s easiest to store them in the order in which they were inserted, since then inserting a new row just means appending to each of the column files.

However, we can choose to impose an order and use that as an indexing mechanism. For example, if queries often target date ranges, such as the last month, it might make sense to make date_key the first sort key. A second column can determine the sort order of any rows that have the same value in the first column. For example, if date_key is the first sort key in Figure 3-10, it might make sense for product_sk to be the second sort key so that all sales for the same product on the same day are grouped together in storage. That will help queries that need to group or filter sales by product within a certain date range.

Different queries benefit from different sort orders, so why not store the same *data sorted in several different ways*? Having multiple sort orders in a column-oriented store is a bit similar to having multiple secondary indexes in a row-oriented store.

Column-oriented storage, compression, and sorting all help to make those read queries faster. However, they have the downside of making writes more difficult. All writes first go to an in-memory store, where they are added to a sorted structure and prepared for writing to disk. When enough writes have accumulated, they are merged with the column files on disk and written to new files in bulk. ( Similar to LSM-Trees )

---

### Materialized views and data cubes

Data warehouse queries often involve an aggregate function, such as COUNT, SUM, AVG, MIN, or MAX in SQL. If the same aggregates are used by many different queries, it can be wasteful to crunch through the raw data every time. Why not cache some of the counts or sums that queries use most often? There is a way to do it - materialized view. *Materialized view* is an actual copy of the query results, written to disk.  When the underlying data changes, a materialized view needs to be updated, because it is a denormalized copy of the data. The database can do that automatically, but such updates make writes more expensive.

A common special case of a materialized view is known as a data cube.
It is a grid of aggregates grouped by different dimensions.
![[DB_Warehouse_Data_cube.png]]

The advantage of a materialized data cube is that certain queries become very fast because they have effectively been precomputed. For example, if you want to know the total sales per store yesterday, you just need to look at the totals along the appropriate dimension—no need to scan millions of rows.

The disadvantage is that a data cube doesn’t have the same flexibility as querying the raw data. For example, there is no way of calculating which proportion of sales comes from items that cost more than $100, because the price isn’t one of the dimensions.

---

