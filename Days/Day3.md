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

### **<u>Cross Product</u>**
* Evaluating joins involves combining two or more relations.
* Given two relations, S and R, each row of S is paired with each row of R.

Example:
```SQL
SELECT drinker
FROM Frequents, Sells
WHERE beer = 'Bud' AND Frequents.bar = Sells.bar;
```

In general:

||Table A|Table B|A X B|
|:-:|:-:|:-:|:-:|
|Rows|N|M|N * M|
|Columns|C|K|C + K|









## **<u>Triggers</u>**