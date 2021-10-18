# Reviewing CSC343 - Part 1

## **<u>Integrity Constraints</u>**
An **integrity constraint (IC)** is a property that must be satisfied by all meaningful database instances

A constraint can be seen as **a predicate**
* A database is **legal** if it satisfies all integrity constraints

Types of constraints:
1. **Intra-relational constraints**
   * i.e. Domain constraints and tuple constraints
   * Avoids entry errors
   * e.g. `(GPA ≤ 4.0) AND (GPA ≥ 0.0)`

2. **Inter-relational constraints**
    * The most common is referential constraint
      * The reference from a row in one table to another table must be valid
      * i.e. No dangling refreences exist!



## **<u>Keys</u>**
A **Key** is a set of attributes that uniquely identifies tuples in a relation

**More Precisely:**
* A <u>set</u> of attributes K is a **superkey** for a relation R if the tuple can be unique identified via the superkey
    * e.g. {SIN}, {SIN, name}, {SIN, name, phone number}, etc.
* A **candidate key** for R is a <u>minimal</u> superkey
* **Primary key:** A candidate key that is arbitrarily selected to be used to uniquely identify the tuples in a relation
* A **foreign key** is a field (or collection of fields) in one table that uniquely identifies a row of another table **or** the same table

### **Keys and Null Values**
The presence of NULL values negatively impacts keys:
* unique identification is no longer guaranteed
* no assistance in establishing correspondence between data in different relations

## **<u>ER Diagrams</u>**

Refer to [this cheatsheet (pdf)](../References/ERD%20Chen%20Notation.pdf) for Chen notation

### **Relationship types**
* Many-Many
  * e.g. <d1 style="color:limegreen;">Sells</d1> between <d1 style="color:dodgerblue;">Bars</d1> and <d1 style="color:dodgerblue;">Beers</d1>
* Many-One
  * e.g. <d1 style="color:limegreen;">Favourite</d1> from <d1 style="color:dodgerblue;">Drinkers</d1> to <d1 style="color:dodgerblue;">Beers</d1>
* One-One
  * e.g. Relationship <d1 style="color:limegreen;">Best-Seller</d1> between entity-sets <d1 style="color:dodgerblue;">Manufacturers</d1> and <d1 style="color:dodgerblue;">Beers</d1>

### **Weak Entity Sets**
Occasionally, entities of an entity set need “help” to identify them uniquely.

Let E represent an entity set; E is said to be <d1 style="color:red;">weak</d1> if:

* E is uniquely identifiable by <u>more than one</u> many-one relationships from E

and  

* E includes the key of the related entities from the connected entity sets

#### **For example:**
* <d1 style="color:darkorchid;">name</d1> is almost a key for football players, but there could potentially be two players with the same name.

* <d1 style="color:darkorchid;">number</d1> is certainly not a key, since players on two teams could have the same number.

* But <d1 style="color:dodgerblue;">Teams</d1> <d1 style="color:darkorchid;">name</d1> and <d1 style="color:darkorchid;">number</d1> (i.e. {name, number}) with relation to the player by <d1 style="color:limegreen;">Plays-on</d1> is unique.

### **Design Techniques**
1. Avoid Redundancy.
2. Limit the use of weak entity sets.
3. Don’t use an entity set when an attribute will do.

