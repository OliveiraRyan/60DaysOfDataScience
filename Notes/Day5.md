# Reviewing CSC343 - Part 4

## <u>**Relational Algebra**</u>
Relational algebra is a system which is used to perform actions within a query language. 

Refer to [my lecture notes](../References/CSC343%20Lec07%20-%20Relational%20Algebra.md) for everything pertaining to relational algebra.

## <u>**Views and Indexes**</u>
### **<u>Views</u>**
More often than not, it is not desirable for all users to see the entire data
instance.
* A **view** provides a mechanism to hide certain data from the view of certain users

Levels of abstraction:
1. Database
2. Physical Schema
   * Describing the files and index(es) used.
3. Conceptual (Logical) Schema
   * Defining the logical structure.
4. Views
   * Describe how users can see the data.

A **view**is a relation defined in terms of stored tables (i.e. **base tables**) and other views
* Two types
  1. **Virtual:** not stored in that database; it is a query for constructing the relation each time they are accessed.
  2. **Materialized:** actually constructed and stored; updated periodically depending on the query definition.

**Declaring views**
```SQL
CREATE [MATERIALIZED] VIEW <name> AS <query>;
```
* the DEFAULT view is VIRTUAL

e.g. 
```SQL 
CREATE VIEW CanDrink AS
    SELECT drinker, beer
    FROM Frequents, Sells
    WHERE Frequents.bar = Sells.bar;
```

**Accessing a View**
Query a view as if it were a *base table*
* Note that there is a limited ability to modify views

e.g.
```SQL
SELECT beer FROM CanDrink
WHERE drinker = ‘Sally’;
```

#### **Updates on Views**
<u>Virtual Views:</u>
* It is impossible to modify a virtual view; because it does not exist
  * Even by using triggers, there is not always a unique translation to the underlying tables

<u>Materialized Views:</u>
* Materialized views are actually constructed and stored (keeping a temporary table)
  * If a view is used frequently enough, it may be more efficient to materialize it!

Concerns: 
* Maintaining correspondence between the base table and the view
when the base table is modified in any manner. 
  * This is computationally costly.

Strategies:
1. Incremental Update
   * Insertions, deletions, and updates to a base table can be implemented in a join view by a small number of queries to the base tables followed by modification statements on the materialized view.
2. Periodic Reconstruction
   * Reconstructing the materialized view which is "out-of-date".

### **<u>Indexes</u>**

**Index:** data structure used to speed access to tuples of a relation, given values of one or more attributes.
* These can be **Hash Tables**, but in a DBMS it is always a balanced search tree with giant nodes (a full disk page) called a **B-tree**.

A major problem in making a database 'run fast' is deciding which indexes to create.
* **Pro:** an index speeds up queries that can use it.
* **Con:** an index slows down all modifications on its relation because the index must be modified too.
* Hand tuning is too hard, so often DBAs use an advisor which gets a **query
load**

Often, the most useful index we can put on a relation is an index on its key
(as it will be utilized frequently).