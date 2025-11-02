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


# ⭐️5. List Manipulation
[`#`](https://code.kx.com/q/ref/take/) (referred to as [take](https://code.kx.com/q/ref/take/)) is used to retrieve the first or last N elements of a list.
```q
L
3#L
-3#L
/
10 20 30 40 50
10 20 30
30 40 50
\

```

Alternatively, by using the keyword [`_`](https://code.kx.com/q4m3/A_Built-in_Functions/#a20-cut-_) (referred to as [cut/drop](https://code.kx.com/q4m3/A_Built-in_Functions/#a20-cut-_) ), we can remove the first or last N elements of a list to return just the remainder of the list.
```q
2_L
-2_L
/
30 40 50
10 20 30
\
```

[`sublist`](https://code.kx.com/q/ref/sublist/)
```q
2 3 sublist 1 2 3 4 5
2 6 sublist "KDB is fun"
/
3 4 5
B is f
\
```

difference between # and sublist
```q
8#til 5
8 sublist til 5
8 sublist 5
/
0 1 2 3 4 0 1 2
0 1 2 3 4
5
\
```

### ⭐️Ex
`The second from last item of L
`The next 2 items starting from the 2nd item from L`
```q
first -2#L  //same as 1#-2#L
(-2#L) 0         //using indexing to get the first of the last two items
L[-2 + count L]  //using indexing directly on L to move 2 back from the end of the list


1 2 sublist L   //using sublist 
L[1+ til 2]     //using direct indexing 
2#1_L           //dropping the first item then taking the first 2
```

Finding item in lists
Using ? but different from the random num generator one.
? is to find ==first== one
```q
//Different from these
?[5;1 4 5 5 6 7]   //generate 5 items drawn randomly from the passed list
5?1 4 5 5 6 7      //infix - generate 5 items from the list 

/
5 7 5 5 7
7 5 6 4 4
\

?[1 4 5 5 6 7;5]     //first occurance of 5
1 4 5 5 6 7?5        //infix - where is the first occurance of 5 in this list
/
2
2
\
?[1 4 5 5 6 7;5 1]   //first occurance of 5 and 1 
1 4 5 5 6 7?5 1 
/
2 0
2 0
\

```
where
```q
where 101010b
/
0 2 4
\
show L:10?50
L >30
where L >30
/
0000101001b
4 6 9
22 6 28 9 31 29 38 10 21 38
\
L[where[L>=30]]
/31 38 38
```

### ⭐️Ex
`Return the items of L that are divisible by 3.`
```q
//reverse way to think. Not to seek one by one. creating boolean list
L
0 = L mod 3
L where 0 = L mod 3
/
22 6 28 9 31 29 38 10 21 38
0101000010b
6 9 21
\
```
`Given the list 1 1 2 3 1 1 2 3, find The index of first occurrence of 3`
```q
ListA:1 1 3 1 1 2 3
ListA?3
/
2
\
//All index occurrences of 3?
where ListA=3
/
2 6
\
// Regarding return type, ? always return atomic and = always return list
```
ate the value of every second item to be null.`
 ```q
show a:10 20 1 15 102 3 8 40 3e
show i:1+2* til div[count a;2]   //getting the odd indexes - div is integer division
/
This would be 3.5 and unexpected input.
show i: 1+2* til count a % 2
\
a[i]:0Ne      //updating - have to use the real type null as the list itself is typed
a
 ```

Amendment using  [`@`](https://code.kx.com/q/ref/amend/)
```q
show L: 10?20
@[L; 1 2]
/
8 17
17 8 17 19 12 14 2 18 16 13
\
@[L; ::; neg]    //amend the list to apply single argument function 
@[L; ::; + ; 2]  //amend the list at the null index (all indexes) to add 2 to each 
L 
/
-17 -8 -17 -19 -12 -14 -2 -18 -16 -13
19 10 19 21 14 16 4 20 18 15
17 8 17 19 12 14 2 18 16 13
\

L
show L:@[L;0 1;10+] //add 10 to the first two ind
```q
show L:30 0N 10 20 50 30 60 30 40 50
maxs L
mins L  
avgs L 
sums L 
/
30 30 30 30 50 50 60 60 60 60
30 30 10 10 10 10 10 10 10 10
30 30 20 20 27.5 28 33.33333 32.85714 33.75 35.55556
30 30 40 60 110 140 200 230 270 320
30 0N 10 20 50 30 60 30 40 50
\
```

### ⭐️Ex
`Create a string list t6 similar to above saying-- ("Welcome";"to";"KDB").
`Using the built-in functions above, deduce and return the word with the most letters.`
`HINT: (you may wish to use the iterator each to apply count to every item in the list)``
```q
/x
t6 where max count each t6
/o
t6 where c = max c:count each t6
```

# 6. List comparison
```q
`b`b`c=(`a`b`c)
(1;2;3;4;5)=(5;2;3;2;1)
/
011b
01100b
\

```

# 7. 
### ⭐️❤️Ex
`Given the list a:10 20 1 15 102 3 8 40 3e of reals, updices in list x
```q
@[L;where L>25;10+] //add 10 to any indice in list x which is greater 25
//same as @[L;where L>25;+;10]
/
13 25 41 38 6 27 25 46 30 32
23 45 51 48 6 37 25 56 40 42
23 35 41 38 6 27 25 46 30 32
\
```
_The identity `::` in kdb+/q serves two purposes - it is both a [generic null](https://code.kx.com/q/ref/identity/#null) (for any type) and also as an identity function (see Functions section). In the example above we are using it as the generic null to indicate we selected no indexes in particular. The following statements are equivalent 
- `L[::]`
- `L[]`
- `L`

?
What would happen if we didn't use the null and instead just called `@[L; ; + ; 2]`?
>> We would create a projection! Similar to `+[;2]` or `mavg[2;]`

Add 300 to every item in the list L that is [`within`](https://code.kx.com/q/ref/within/) the range 10-15
```q
@[L;where L within 10 15; 300+]
L within 10 15
where L within 10 15
L
/
8 36 23 37 36 4 4 310 49 23
0000000100b
,7
8 36 23 37 36 4 4 10 49 23
\
//realized this is not where for condition where for finding the place
```

Amendment using [`.`](https://code.kx.com/q/ref/apply/#apply-index)(aka [apply-index](https://code.kx.com/q/ref/apply/#apply-index)).
```q
matrix
matrix . 1 2     // same as matrix[1;2]
matrix . (::;0)   // same as matrix[::;0] //::meaning all
/
1 2 3
4 5 6
6
1 4
\
.[matrix; 0 1]             //equivalent to matrix[0;1]
.[matrix; 0 1; : ; 500 ]   //we can now update at depth
matrix
/
2
1 500 3
4 5   6

1 2 3
4 5 6
\
```

### ⭐️Ex
Use indexing at depth on ("nuf";"BDK";" si") to make

"KDB"
"is "
"fun"
```q
("nuf";"BDK";" si") . (1 2 0;2 1 0)
.[("nuf";"BDK";" si");(1 2 0;2 1 0)]  //functional syntax
/
"KDB"
"is "
"fun"
\
```


# Practical Guidance
## PG1 enlist vs (),
- **`enlist` always wraps the value one more level** (creates an additional layer of nesting).
- **`(),x` guarantees the result is a list** without increasing the nesting level of an existing list.
- When in doubt and you **don’t want to introduce extra nesting**, use **`(),x`** to avoid unexpected structures.
```q
a: 1 2          / 1 2 is a list of longs (type 7h)
enlist a
// ,1 2          Becomes a nested list: a list containing the list 1 2
(),a
// 1 2           Remains the same structure as the original list
type enlist a    / Since it's now a list of lists, it becomes a general list = 0h
// 0h
type (),a       / Keeps the original type (here, long list) = 7h
// 7h
```

## PG2 Fill Operator
```q
show g:1 2 0N 0N 3 4 0N
1^g                          //replace any nulls with 1
/
1 2 1 1 3 4 1
1 2 0N 0N 3 4 0N
\
avg[g]^g
//1 2 2.5 2.5 3 4 2.5

//replace with list
g                                 //list with some null values 1 2 0N 0N 3 4 0N
show k:10 20 30 40 50 60 70       //list without null values
k^g                               //Replace nulls in g with corresponding values from k
/
1 2 0N 0N 3 4 0N
1 2 30 40 3 4 70
10 20 30 40 50 60 70
\

//replcae with previous non null value
show g:1 2 0N 0N 3 4 0N
fills g
/
1 2 0N 0N 3 4 0N
1 2 2 2 3 4 4
\
//To replace with following non null value
reverse fills reverse g
//1 2 3 3 3 4 0N
```

## PG3 Textual data
list of list is general list.
"string" is char list so string list is general list
```q
type A:("a";"a")
type AB:("a";"ab")
/
10h
0h
\
```

# PG4 Common error debug
to check the length of the lists, using bracket for multiple input for func `each`
```
count each (l1;l2)
```

list type error
```q
l1
@[l1;2;%;2] //amend list 1 at index 2 to divide by 2  - fails because resultant type is a float 
/
0 1 2 
QError: type
\
//These can work
@[l1;2;div;2]
0 1 1
```


