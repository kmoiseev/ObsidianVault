Well-chosen indexes speed up read queries, but every index slows down writes.

Key-value indexes (B-Tree/LSM-Tree) are like a primary key index in the relational model. A primary key uniquely identifies one row in a relational table, or one document in a document database, or one vertex in a graph database. Other records in the database can refer to that row/document/vertex by its primary key (or ID), and the index is used to resolve such references.

 A secondary index can easily be constructed from a key-value index. The main difference is that keys are not unique; i.e., there might be many rows (documents, vertices) with the same key. This can be solved in two ways: either by making each value in the index a list of matching row identifiers or by making each key unique by appending a row identifier to it. Either way, both B-trees and log-structured indexes can be used as secondary indexes.
 
 The key in an index is the thing that queries search for, but the value can be one of two things: it could be the actual row in question, or it could be a reference to the row stored elsewhere. In the latter case, the place where rows are stored is known as a *heap file*, and *it stores data in no particular order*. 
 
 When updating a value without changing the key, the heap file approach can be quite efficient: the record can be overwritten in place, provided that the new value is not larger than the old value. The situation is more complicated if the new value is larger, as it probably needs to be moved to a new location in the heap where there is enough space. In that case, either all indexes need to be updated to point at the new heap location of the record, or a forwarding pointer is left behind in the old heap location.
 
 In some situations, the extra hop from the index to the heap file is too much of a performance penalty for reads, so it can be desirable to store the indexed row directly within an index. This is known as a *clustered index*.
 
 A compromise between a clustered index and a nonclustered index is known as a *covering index* or *index with included columns*, which stores some of a table’s columns within the index. This allows some queries to be answered by using the index alone.
 
 As with any kind of duplication of data, clustered and covering indexes can speed up reads, but they require additional storage and can add overhead on writes. Databases also need to go to additional effort to enforce transactional guarantees, because applications should not see inconsistencies due to the duplication.
 
 The most common type of multi-column index is called a *concatenated index*, which simply combines several fields into one key by appending one column to another. The index (first_column, second_column) can be used to find all the object with particular first_column or particular first_column, second_column. Bot not for searching by second_column.
 
Multi-dimensional indexes are a more general way of querying several columns at once, which is particularly important for geospatial data.
For example, query 
```SQL
SELECT * FROM restaurants WHERE latitude > 51.4946 AND latitude < 51.5079 AND longitude > -0.1162 AND longitude < -0.1004;
```
 One option is to translate a two-dimensional location into a single number using a space-filling curve, and then to use a regular B-tree index. More commonly, spe‐ cialized spatial indexes such as R-trees are used. For example, PostGIS implements geospatial indexes as R-trees using PostgreSQL’s Generalized Search Tree indexing facility. On an ecommerce website you could use a three-dimensional index on the dimensions (red, green, blue) to search for products in a certain range of colors.