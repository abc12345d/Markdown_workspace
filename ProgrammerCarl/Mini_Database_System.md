# Mini database system

A lightweight relational database system that handles conjunctive query (subset of SQL), has query optimisation and supports basic data operations such as scanning, selection, projection, join and aggregation. 

---

This is the coursework for Advanced Database System (INFR11199). The skeleton project comes with a query parser and my job is implementing:

1. the minimization of query 
2. the interpreter of query

## The minimization of query 
Any simple SELECT-FROM-WHERE SQL query (only AND and equality in the WHERE clause) can be expressed as a conjunctive query (CQ). For example,

```
Q(x, SUM(t)) :- R(x, y, z), S(x, w, t), x >= 5

is equivalent to

SELECT x, SUM(t)
FROM r
INNER JOIN S ON R.x = S.x
WHERE x >= 5
```

By minimising the CQ, we can minimise the number of relations which will be passed to the interpreter of CQ, resulting to a significant reduction in time complexity. For example (1),

```
Q(x) :- T(x, y, z), T(x, t, 2), T(w, t, z), T(x, t, z)

can be minimised to 

Q(x) :- T(x, yt, 2)

```

the working process of minimization is
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
<img width="888" alt="backtrackingTree" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/1a83e81d-3d7b-415f-b854-89ce0e98ac62">

