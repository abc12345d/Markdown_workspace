# Mini database system

A lightweight relational database system that handles conjunctive query (subset of SQL), has query optimisation and supports basic data operations such as scanning, selection, projection, join and aggregation. 

---

This is the coursework for Advanced Database System (INFR11199). The skeleton project comes with a query parser. My role involves developing a program that takes in a database (a set of CSV files with data) and an input file containing one query. The program then processes and evaluates the query on the database and writes the query result in the specified output file.

We can separate the program into two main parts: 
1. the minimization of query 
2. the interpreter of query 

## The minimization of query 
By minimising the conjunctive query (CQ, see further notes for more details), we can minimise the number of relations which will be passed to the interpreter of CQ, resulting to a significant reduction in time complexity. For example (1),

```
Q(x) :- T(x, y, z), T(x, t, 2), T(w, t, z), T(x, t, z)

can be minimised to 

Q(x) :- T(x, yt, 2)

```
To me, the hardest part in query minimization is transforming working process of CQ minimization into actual algorithm.

The working process of minimization is
1. x appeared in query head so it is a must-variable that cannnot be deleted 
- confirmed set: {x = x}
- stage set: {}
2. first relation and second relation have matched attribute `x` in same position so `y = t` and `z = 2`. The `2` is a constant hence `z = 2` is also added to confirmed set.
- confirmed set: {x = x, z = 2}
- stage set: {y = t}
3. first relation and third relation have matched attribute `z` in same position so `w = x` and `t = y`. 
- confirmed pair: {x = x, z = 2}
- stage set: {y = t, w = x, t = y}
4. resolve `y = t` and `t = y` and move the new pairing into the confirmed set
- confirmed set: {x = x, z = 2, y = yt, t = yt}
- stage set: {w = x}
5. second relation and last relation have matched attribute `x` and `t` so `z = 2`
- confirmed set: {x = x, z = 2, y = yt, t = yt}
- stage set: {w = x}

The pseudocode for query minimization is:

```
1. form confirmed_set
   - map variables in the query head to themselves
   - map constants to themselves and preprocess body of query to do CQ minimisation
3. store relational atoms with same name into an arraylist. 
   E.g. R(a,b,c) and R(c,d,e) will be stored to same arraylist and 
        S(f,g) and S(h,i) will be stored in another arraylist
5. find all the available substitution for every relational atoms by finding pairing (R_1, R_2) where 
   E.g. R_1(a,c,c) and R_2(x,y,z)
        available substitution is [x = a, y = c, z = c] from R_2 to R_1
6. use backtracking algorithm to find the correct substitution for each relational atom where a correct
   substitution will not clash with the confirmed_set
```

Below is the backtracking tree for the example (1):
<img width="555" alt="backtrackingTree" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/1a83e81d-3d7b-415f-b854-89ce0e98ac62">

## The interpreter of query


There are many processing models that can be used by a database system for query execution. For this database system, my processing model is the iterator model. This model involves integrating a `getNextTuple()` function within each operator present in the query tree. Besides, there are also various ways to organise a query tree, each comes with a different cost. To achieve database optimisation, I strategically arranged the query tree to minimize costs:
<img width="442" alt="query tree" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/9a30a224-e76e-4cf5-993f-dda9c4a7981e">

In summary, I implement:
1. basic components which form a query (see further notes for UML class diagram)
2. iterator model for operators
   - Scan Operator
   - Selection Operator
   - Projection Operator
   - Join Operator
   - Sum Operator
   - Avg Operator
3. a query tree using those operators (see further notes for UML class diagram)
   - a left-deep join tree if there are more than 1 Join Operator

## Final Outcome of the Database System
Given the below database schema and relations:
<img width="666" alt="database_schema" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/8ae6d46f-d25e-495d-9c62-5db58dd2c331">

The query and its output data:
<img width="666" alt="Output_data" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/0d0eaa28-a879-4dfa-91de-db0b0a6629ab">

# Further notes

## What is CQ?
Any simple SELECT-FROM-WHERE SQL query (only AND and equality in the WHERE clause) can be expressed as a conjunctive query (CQ). For example,

```
Q(x, SUM(t)) :- R(x, y, z), S(x, w, t), x >= 5

is equivalent to

SELECT x, SUM(t)
FROM r
INNER JOIN S ON R.x = S.x
WHERE x >= 5
```

## UML class diagram for query components
<img width="666" alt="query component" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/f62bef94-254f-4e08-aa11-03e1f0240ad0">

## UML class diagram for query tree
<img width="666" alt="QueryPlan" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/6fe61a1a-394e-40f8-a731-dce1ec2841ca">
