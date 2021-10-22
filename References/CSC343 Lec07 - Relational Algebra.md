# CSC343 Lec07: Relational Algebra

Query Languages allow manipulation and <u>retrieval of data</u> from a database
* The relational model supports simple and powerful Query Languages
* Query Languages != Programming Languages
    * Are not intended for complex calculations
    * Support easy and efficient access to large data sets

## What is Algebra
In general, it is a system that consists of:
* **Operands**: variables or values from which new values can be
constructed.
* **Operators**: symbols which denote procedures that construct
new values from inputted/given values.

## What is Relational Algebra?
It is an algebra whose operands are relations or variables that represent relations.
* The operators are designed to perform the most common
operations that users need to do with relations in a database
    * The result is an algebra that can be used as a Query Language for relations.

#### To clarify:
* Operands: 
    * tables (relations)
* Operators:
    * Regular set operators (union, intersection, difference)
    * Choose only rows you want (selection)
    * Choose only columns you want (projection)
    * Combine tables (join)
    * etc.

## Base Operators 
* $σ$ is Selection
    * Selection used to specify a certain <u>row</u>
    * $σ_c$ (R) where c is a list of conditions involving the attribute(s) R.
* $π$ is Projection
    * Selection used to specify a certain <u>column</u>
    * $π_φ$(R) where φ is a list of attributes involving the attribute(s) R.
* x is Cartesian Product
    * Cartesian Product is the combination of two (or more) relations.
    * R := $R_1$ x $R_2$ where the tuples of $R_1$ and tuples $R_2$ are paired together to form R.
* $∪$ is Union
    * Combine 2 relations into 1
* $∩$ is Intersection
    * Used to see what overlaps across 2 relations
* $−$ is Difference
    * Basically subtraction - used to see what is left over in $R_1$ when $R_1 - R_2$   
  
## Extended Relational Algebra
* $⋈$ is Natural Join
    * Connects (joins) 2 relations by equating attributes of the same name and projecting out 1 copy of each pair of equated attributes
* $⋈_c$ is Theta Join
    * Where condition c is either:
        * a $θ$ b, where a and b are *attribute names*
        * a $θ$ x, where a is an *attribute name* and x is a *value*
    * $⋈_c$ is Equijoin <u>iff</u> c's $θ$ is the equality operator (i.e. $=$)
* $ρ$ does renaming
    * $R_1 = ρ_{R_1(A_1,..,A_n)}(R_2)$, where $R_1$ becomes a new relation with the attributes $A_1,..,A_n$ of $R_2$
    * $ρ_{a/b}(R)$, where b is an attribute of $R$ and a is the renamed attribute name
* $δ$ does Duplicate Elimination
    * This is used for duplicate elimination in <u>bag semantics</u>
    * $R_1 = δ(R_2)$ will result in $R_1$ containing 1 copy of each tuple that appears in $R_2$ 1 or more times
*  $τ$ is the Sorting of Tuples
    * $R_1 = τ_L(R_2)$, where $L$ is a list of of$R_2$'s attributes
    * $R_1$ is the sorted list of tuples of $R_2$, based on the order specified in list $L$
        * Ties are broken arbitrarily
    * Items sorted in ascending order by default
        * If you want to sort by descending, you will denote the list with prefix $-$
            * e.g. $R_1 = τ_{-L}(R_2)$
    * <u>**Fun Fact:**</u> $τ$ is the <u>only</u> operator whose result is neither a set nor a bag
* $γ$ is Grouping and Aggregation
    * The aggregation operators are:
        * AVG
        * MIN
        * MAX
        * SUM
        * COUNT
    * $R_1 = γ_L(R_2)$, where $L$ is a list of individual grouping attributes or <u>an</u> aggregation operation of an attribute
    * **Note:** An arrow to a new attribute name would rename it
        * e.g. AVG(salary) → pay

## RA Rules of Precedence
1. [σ, π, ρ]
2. [x, ⋈]
3. [∩]
4. [∪, −]

> Note: you may combine operators with parentheses and precedence rules.

## Bag vs. Set Semantics
* A bag (aka multiset) allows duplication
* A set is a collection of distinct objects
* SQL is a bag language
* <u>Fun Fact:</u> some operations, like projection, are more efficient on bags than sets.

Why bags?
* Primarily due to performance
    * Getting rid of duplicates can be computationally expensive

#### Bag Laws != Set Laws

* The commutative law for ∪ does hold for bags.
    * i.e. $R_1$ ∪ $R_2$ = $R_2$ ∪ $R_1$
* The idempotent law, in general, for ∪ does not hold for bags.
    * i.e. $R_1$ ∪ $R_1$ ! = $R_1$
    * e.g. {1} ∪ {1} = {1, 1} ! = {1}

#### Union Compatibility
∪, ∩, and −, when used on bags, turn them into sets

## Expression Trees
* Leaves are operands – either variables standing for relations or particular, constant relations.
* Interior nodes are operators – applied to their children.
* We traverse the tree from leaves-to-root (and apply the logic at each node)

