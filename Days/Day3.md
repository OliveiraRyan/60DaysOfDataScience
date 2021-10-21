# Reviewing CSC343 - Part 3

## <u>**Aggregation**</u>

### **<u>Aggregation Operators</u>**
Column values are calculated, and a single value is returned.
* Aggregation Operations are
Column Operations!
* SUM, AVG, COUNT, MIN, and MAX.
  * Applied on a SELECT clause in a query
* To eliminate duplicates use DISTINCT inside an aggregation.
* NULL values are **NEVER** contributed to an aggregate operation


### **<u>Grouping</u>**

We may follow a SELECT-FROM-WHERE expression by **GROUP BY** and a list
of attributes.

The relation that results from the SELECT-FROM-WHERE is grouped
according to the values of all those attributes, and any aggregation is
applied only within each group.

Example: Find the average price for each beer from <d1 style="color:hotpink;">Sells(bar, beer, price)</d1>
```SQL
SELECT beer, AVG(price)
FROM Sells
GROUP BY beer;
```

If any aggregation is used, then each element of the **SELECT** list must be either:
1. Aggregated, or
2. An attribute on the **GROUP BY** list

**Illegal Query Example:**
```SQL
SELECT bar, beer, MIN(price)
FROM Sells
GROUP BY bar;
```
<img src="../Images/Day3/Sells_Aggregation.png"/>
<img src="../Images/Day3/Sells_Aggregation_Result.png"/>

**Solution:**  
Use a join or subquery. Here's an example using a subquery:
```SQL
SELECT bar, beer, price
FROM Sells
WHERE (beer, price) IN (
    SELECT beer, MIN(price) 
    FROM Sells
    GROUP BY beer
    );
```


**HAVING Clauses**  

* **HAVING \<condition\>** may follow a **GROUP BY** clause.
  * If so, the condition applies to each group, and groups not satisfying the
condition are eliminated.

Example:
```SQL
SELECT beer, AVG(price)
FROM Sells
GROUP BY beer
HAVING COUNT(bar) >= 3 OR
    beer IN (
    SELECT name
    FROM Beers
    WHERE manf = ‘Pete”s’
    );
```

## **<u>Joins</u>**

### **<u>Cross Joins</u>**
* Evaluating joins involves combining two or more relations.
* Given two relations, S and R, each row of S is paired with each row of R.

Example:
```SQL
SELECT drinker
FROM Frequents, Sells
WHERE beer = 'Bud' AND Frequents.bar = Sells.bar;
```
* Note: may also be executed using **CROSS JOIN**

In general:

||Table A|Table B|A X B|
|:-:|:-:|:-:|:-:|
|Rows|N|M|N * M|
|Columns|C|K|C + K|

### **<u>Types of Joins:</u>**

#### **Outer Joins:**
* An extension of the join operation that avoids loss of information.
* Computes the join and then adds tuples from one relation that does not match tuples in the other relation to the result of the join.
  * Preserves dangling tuples by padding them with NULL.

**R OUTER JOIN S** is the core of an outer join expression.  
Modified by the following:
1. Optional **NATURAL** in front of **OUTER**.
   * Check equality on all common attributes.
   * No two attributes with the same name in the output.
2. Optional **ON \<condition\>** after **JOIN**.
3. Optional **LEFT**, **RIGHT**, or **FULL** in front of **OUTER**.
   * **LEFT** = pad dangling tuples of R only.
   * **RIGHT** = pad dangling tuples of S only.
   * **FULL** = pad both; this is the DEFAULT choice.

#### **Inner Joins**
* **INNER** keyword is optional, since the default **JOIN** operates as an inner join
  * i.e. **INNER JOIN** == **JOIN**
  * Although functionally equivalent, **INNER JOIN** may be a bit clearer to read

Unlike outer joins, does not add dangling tuples to the result of the join.  
Modified by the following:
1. Optional **ON \<condition\>** after **JOIN**.

e.g.
```SQL
SELECT *
FROM Course INNER JOIN Prereq 
ON Course.course_id = Prereq.course_id
```

## **<u>Triggers</u>**

A procedure that is automatically invoked by the DBMS in response to specified changes to the database.
* Typically invoked by the DBA.

#### **A trigger description contains three parts:**
1. **Event:** a change to the database activates the trigger.
2. **Condition:** a query or test that is run when the trigger is activated.
   * Can be a Boolean expression or a query
     * A query is interpreted as TRUE if the answer set is non-empty  
3. **Action:** a procedure that is executed when the trigger is activated and its condition is true.
   * We must specify at what point in the sequence of events it occurs
     * We may want to modify if the action occurs *before* or *after* changes are made to relations

Types of triggers:
* BEFORE
  * Occurs before executing the statement
* AFTER
  * Occurs after executing the statement
* INSTEAD OF
  * Executes trigger instead of executing the statement

#### **Row-Level Trigger vs. Statement-Level Trigger**
* A statement-level trigger is activated once.
* A row-level trigger is activated once per iteration of the loop.
  * e.g. UPDATE attribute
  * SET columnOne = columnOne +1;