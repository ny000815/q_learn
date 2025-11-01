# 1.  Intro
Lists are the basis of all data structures in kdb+. For example, a dictionary is a collection of lists and a table is a list of dictionaries. As kdb+ is a column orientated database and a column is really a list, the list data structure is of great importance in this technology.

One very powerful feature of q is the ability to execute the built in arithmetic and logical operators directly on lists without the need for some explicit loops across the list.


# 2. Simple Lists (= Vector) & General Lists
`Create a 10 digit integer list called t1 (using casting & til `

```q
show t1: `int$til 10
```

`Create a general list t2, comprising of a list of lists. The first list should be a list of floats from 1 to 3, and the second should be a list of the symbols `a, `b, and `c. Confirm the type of t2.`
```q
show t2:(1 2 3f; `a`b`c)
type t2
type each t2
/
0h
9 11h
1 2 3
a b c
\
```

`Create a single item list of the symbol `onMyOwn`
```q
show sList:(),`onMyOwn
```

# 3. List creation
generate numbers without replacement
```q
?[-5;10]
-5?10
/
7 3 6 2 1
1 9 6 4 0
\
?[-11;10]
//this would be error :length
```
date version
```q
?[5;2011.01.01-til 3]  //select 5 elements from a list of 3 dates
5?2011.01.01-til 3
/
2010.12.30 2010.12.30 2010.12.31 2010.12.30 2010.12.31
2010.12.30 2011.01.01 2011.01.01 2010.12.31 2011.01.01
\
```

# 4. List indexing and access
The type of the null value at out bound of general list is defined by the first item's type
```q
show GList:(1;2;`Real)
GList 10
show GList2:(3e;1)
GList2 4
/
0N
0Ne
1
2
`Real
3e
1
\
```

cut & matrix
```q
show matrix: 4 cut 3* til 9 
matrix[1]    //the first item of our matrix is the first "row"
/
12 15 18 21
0 3 6 9
12 15 18 21
,24
\
matrix[0;1 2] // or matrix[0][1 2]
/
3 6
\

show matrix: 3* 3 cut til 9
matrix[0 1 2;0]
matrix[;0]
`
/
0 9 18
0 9 18

0 3 6
9 12 15
18 21 24
\
```

