### Relational Model
Tables represent a data scheme, rows represent data.
Tables are interconnnected with relations.

---

### Document Model
Data is not linked with each other. Nested records are storeed within their parent record. Structure of every object is usually represented in JSON schema (might be also XML or binary format of JSON/XML). Related item can be reference by a *document reference*.

---

### Network Model (Hieararchical Model ) - not actual
In hierarchical model every object has a single parent. In network model every object has multiple parents. The only way of accessing a record is to follow a path from a root record along these chains of links

---

### Graph-like Data Model
In the model data represented as *nodes* connected with *edges*
Graph relations are more flexible than in relational database. Because you can set up any kind of edges between any kind of nodes.

##### Property graphs
Node consists of:
- Unique id
- Outgoing edges
- Incoming edges
- Key-value properties

Edge consists of:
- Unique id
- Node where edge starts (tail)
- Node where edge ends (head)
- A label to describe the kind of relationship
- Key-value properties

##### Triple-stores
All information is stored inthe form of three-part statements: 
*(subject, predicate, objects)*
For example, 
*(Jim, likes, bananas)*
Object can be:
- a primitive datatype. For example, *(babana, weight, 38)*
- Another subject


---

### Relational vs Document
- Document db schema is more flexible
- Document db structure is closer to the data structures used in OOP applications
- Relational db provides better support for joins, many-to-many, many-to-one relations
- You cannot refer directly to a nested item within a document
- Relational have *schema-on-wtire* (validates data when it is inserted) while document have *schema-on-read* (validates when data is read). The schema-on-read approach is advantageous if the items in the collection don’t all have the same structure for some reason (managed externally, for example)
- Documents must be kept small, because every time documents need an update - db rewrites the whole document (until size of it is not changed). 

---

#### Document Model application
Data can be represented as a tree of one-to-many relationships, where typically the entire tree is loaded once

#### Document Model application examples
- Analytics application which records event occured during specific time

---

#### Relational Model application
Highly interconnected data. SImple many-to-many relations

---

#### Graph-model application
Many-to-many relationships are very common in the data.

#### Graph-model application examples
- Social graps - people are nodes friendships are edges
- Web graph - pages are nodes links are edges
- Road netwroks - junktions are nodes roads are edges


### PS - other models

 **Genom database** - to match a giant string (genom) against a large database of strings that are similar, but not identical. Example - GenBank
 
 **Full-text search** - searching a single computer-stored document or a collection in a full-text database
 