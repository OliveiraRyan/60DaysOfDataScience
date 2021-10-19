# Reviewing CSC343 - Part 2

## <u>**Data Definition Language (DDL)**</u>
**Common Types:**
* INT or INTEGER (synonyms).
* Also: SMALLINT, MEDIUMINT, and BIGINT
* REAL or FLOAT or DOUBLE (synonyms).
* CHAR(n) = fixed-length string of n characters.
* VARCHAR(n) = variable-length string of up to n characters.
  * Wrapped in single quotes
  * Two single quotes = real quote
    * e.g. 'Joe''s Bar'

**Declaring Keys**

Single-Attribute Keys:
```SQL
CREATE TABLE Beers (
name CHAR(20) UNIQUE,
manf CHAR(20)
);
```

Multi-Attribute keys:
```SQL
CREATE TABLE Sells (
    bar CHAR(20),
    beer VARCHAR(20),
    price REAL,
    PRIMARY KEY (bar, beer)
);
```

**Primary Key vs. Unique**
1. There can only be **one** primary key for a relation, but **several** unique attributes
2. **No** attribute of a primary key can ever be NULL in any tuple. But attributes declared unique **may have** NULLs, and there may be several tuples with NULL.

**Expressing Foreign Keys**
```SQL
    CREATE TABLE Beers (
    name CHAR(20) PRIMARY KEY,
    manf CHAR(20)
);

CREATE TABLE Sells (
    bar CHAR(20),
    beer CHAR(20),
    price REAL,
    FOREIGN KEY(beer)
        REFERENCES Beers(name)
        ON DELETE SET NULL
        ON UPDATE CASCADE

);
```
* Note, NO ACTION is equivalent to RESTRICT
  * i.e. rejects the delete/update operation



## <u>**Data Manipulation language (DML)**</u>

### **Operations**
* Rename attribute with **AS**
* Compare a string to a pattern using
  * \<Attribute\> **LIKE** \<pattern\> **or** \<Attribute\> **NOT LIKE** \<pattern\>
    * `%` = "any string"
    * `_` = "any character"
* Check for values between a range (without using **>=** and **<=**)
  * salary **BETWEEN** 90000 **AND** 100000;
* **ANY** operator
  * *x* equals at least one tuple in the subquery result
    * X = **ANY**(\<sub-query\>)
    * **=** could be any other comparison (e.g. **>=**)
* **ALL** operator
  * for every tuple *t* in the relation, *x* is not equal to *t*.
      * x **<>** **ALL** (\<subquery\>)
      * **<>** could be any other comparison (e.g. **>=**)
* **IN** operator
  * the \<value\> is a member of the relation produced by the sub-query
    * \<value\> **IN** (\<subquery\>)
      * Opposite: \<value\> **NOT IN** (\<subquery\>)
    * **<u>Performance Note:</u>** if the joining column is not **UNIQUE** then **IN** is faster than **JOIN** on **DISTINCT**.
  * **<U>Note</u>** that **IN()** is equivalent to **= ANY()**
    * More concise
  * **<u>HOWEVER</u>** **<> ANY** operator differs from **NOT IN**
    * **<> ALL** means the same as **NOT IN**
* **EXISTS** operator
  * **Exists (\<subquery\>)** is true iff the sub-query is **<u>not empty</u>**.


### **Union, Intersection, and Difference**
* **UNION**
  * All unique rows from both queries
  * **UNION ALL** includes duplicates as well
* **INTERSECT**
  * Common unique rows from both queries
* **EXCEPT**
  * Unique rows from left query that aren't in the right query's results

**<u>Note</u>:**
* SELECT-FROM-WHERE statement uses bag semantics
  * Retains duplicates
  * Force the result to be a set by **SELECT DISTINCT…**
* The default for union, intersection, and difference is set semantics
  * Eliminates duplicates
  * Force results to be a bag by **ALL** operator
    * i.e. **… UNION ALL …**


## **Extra things to note:**
**Logic in SQL is 3-pronged:**
1. TRUE
2. FALSE
3. UNKNOWN

**Three types of modifications:**
1. Insert
2. Delete
3. Update

**Simple Self-Join without producing duplicate pairs**
* Do not produce pairs like (Bud, Bud).
* Do not produce the same pairs twice (Bud, Miller) and (Miller, Bud).

```SQL
SELECT b1.name, b2.name
FROM Beers b1, Beers b2
WHERE b1.manf = b2.manf AND b1.name < b2.name;
```


## <u>**Aggregation, Joins, and Triggers**</u>

### **Aggregation Operators**
Column values are calculated, and a single value is returned.
* Aggregation Operations are
Column Operations!
* SUM, AVG, COUNT, MIN, and MAX.
  * Applied on a SELECT clause in a query
* To eliminate duplicates use DISTINCT inside an aggregation.
* NULL values are **NEVER** contributed to an aggregate operation


### **Grouping**
