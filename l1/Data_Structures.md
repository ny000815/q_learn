# 4. Lists

## 4.1 Data types
h is suffix for indicating data types
```q
vtsfares:select fare from trips where date = 2009.01.01, vendor=`VTS
vtsfares
type vtsfares
/
fare
----
5.7 
4.9 
4.9 
..
98h
\

fares: vtsfares`fare
fares
type fares
/
5.7 4.9 4.9 4.5 4.9 15.3 3.7 8.5 13.3 11.3 22.5 6.9 16.5 24.1 9.3 9.3 6.1 8.1..
9h
\

//While simple lists always have strictly positive values returned by type, general lists always have type zero.
general:(`VTS;23.45);
general
type general
/
`VTS
23.45
0h
\

//A list can be _empty_ - if there had been a typo in the select statement, and we inquired about a non-existent cab company, we would see:

//if it is negative, it is scalar type
aaabbbccc: 9.93
type aaabbbccc
/
-9h
\


```

## 4.2 Casting
 `$` used to [cast](https://code.kx.com/q/ref/cast/) an empty list.
[List](https://code.kx.com/q4m3/3_Lists/)
```q
1 2
`float$1 2 //using it's symbol name 
"f"$1 2  //using it's character letter
9h$1 2   //using it's short value
`*
type 1 2
type`float$1 2 
type "f"$1 2  //using it's character letter
type 9h$1 2   //using it's short value
/
1 2
1 2f
1 2f
1 2f
*[`]
7h
9h
9h
9h
\

() //general list 
`long$() //list of type long
/
()
`long$()
\

```

### sublist
```q
fares
/
5.7 4.9 4.9 4.5 4.9 15.3 3.7 8.5 13.3 11.3 22.5 6.9 16.5 24.1 9.3 9.3 6.1 8.1..
\

10 sublist fares
/
5.7 4.9 4.9 4.5 4.9 15.3 3.7 8.5 13.3 11.3
\

-10 sublist fares
/
12.9 4.9 44.5 5.3 19.3 9.3 8.9 45 9.7 4.5
\

//sublist has a feature that the number of elements it returns is _capped_ at the size of the list that it operates on.
count fares
count 10000000 # fares
count 10000000 sublist fares

/
124208
10000000
124208
\



```

### Exercise 9
Use sublist to get the second 10 elements in the list
<details>
  <summary>Answer</summary>

<pre><code>/▲
subA:20 sublist fares
-10 subA subList
/
22.5 6.9 16.5 24.1 9.3 9.3 6.1 8.1 5.7 8.5
\

/o
-10 sublist 20 sublist fares
/
22.5 6.9 16.5 24.1 9.3 9.3 6.1 8.1 5.7 8.5
\
</code></pre>

</details>

### sort
```q
fares
show sortedFares:asc fares
10 sublist sortedFares
-10 sublist sortedFares

/
`s#2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2...
5.7 4.9 4.9 4.5 4.9 15.3 3.7 8.5 13.3 11.3 22.5 6.9 16.5 24.1 9.3 9.3 6.1 8.1..
2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5
104 104 108.5 112.5 120 122 128 134.3 190 200
\
```

## 4.3 Obtaining random data

An easy trap to fall into – we extracted an _unrepresentative sample_. To pick ten _random_ records from the list, we can use the [Roll `?` operator](https://code.kx.com/q/ref/deal/)
```q
sampleFares:10?sortedFares;
sampleFares
sampleFares[8]
/
30.9 4.9 4.1 5.3 8.5 6.1 5.3 4.5 12.9 7.7
12.9
\

fares[10+til 10]
/
22.5 6.9 16.5 24.1 9.3 9.3 6.1 8.1 5.7 8.5
\
```

### ⭐️Exercise 10
Use indexing to find the middle value in the sortedFares list.
<details>
  <summary>Answer</summary>

<pre><code>/x
//Not sure why it needs to be cast. scala type is not suitable?
count sortedFares
type count fares
sortedFares[124208%2]
/
124208
-7h
QError: type
\

/o
sortedFares [`long$(count sortedFares)%2]
/
6.9
\
</code></pre>

</details>

### index
```q
//In the case of a simple list, if the index used is too high, a _null_ of the list’s type is returned.
sortedFares[count sortedFares]
sortedFares[-1+count sortedFares]  // index from 0 to N-1
/
0n
200f
\
//The below code block will also obtain the first value from the list: 
1 sublist sortedFares
first sortedFares
type 1 sublist sortedFares
type first sortedFares
/
,2.5
2.5
9h
-9h
\

/if is a single item from list type, it has "," at first.
enlist 499
(),499
(),499 500
type enlist 499
type (),499
type (),499, 500
/
,499
,499
499 500
7h
7h
7h
\
```

## 4.4 List Amendment

A simple list can be indexed into using the `@` operator:
```q
2* til 5
@[sampleFares;(2*til 5)]
sampleFares[2*til 5] //Same result
/
0 2 4 6 8
30.9 4.1 8.5 5.3 12.9
30.9 4.1 8.5 5.3 12.9
\
```

The `@` operator can be applied with further arguments so that the list can be altered. Below we replace the items at positions `2*til 5` with `99f`.
```q
sampleFares
// index into sampleFares
// using list of indexes (2*til 5)
// assign these values - :
// the value 99f
@[sampleFares;(2*til 5);:;99f]  
@[sampleFares;(2*til 5);+;99f]
@[sampleFares;(2*til 5);]
sampleFares //original list is not updated

/
30.9 4.9 4.1 5.3 8.5 6.1 5.3 4.5 12.9 7.7
99 4.9 99 5.3 99 6.1 99 4.5 99 7.7
129.9 4.9 103.1 5.3 107.5 6.1 104.3 4.5 111.9 7.7
@[30.9 4.9 4.1 5.3 8.5 6.1 5.3 4.5 12.9 7.7;0 2 4 6 8;]
30.9 4.9 4.1 5.3 8.5 6.1 5.3 4.5 12.9 7.7
\

//To persist the change, prefix the name of the list with a back-tick; or assign the result to a name:
test:@[fares;(2*til 4);:;0Nf]
test
@[`fares;(2*til 4);:;0Nf]
fares
```

Extend a list by appending to to it using the [Join operator `,`](https://code.kx.com/q/ref/join/).
```q
fares,:12.34
-10#fares    // inspect the end of the list to see the appended value
/
4.9 44.5 5.3 19.3 9.3 8.9 45 9.7 4.5 12.34
\
```

null
```q
any null fares
where null fares
fares
/
1b
0n 4.9 0n 4.5 0n 15.3 0n 8.5 13.3 11.3 22.5 6.9 16.5 24.1 9.3 9.3 6.1 8.1 5.7..
\
```

### Exercise
```q
/x
fares:@[fares;*2till5;:avg fares;] /place of ";"  and no need to assing og one
/ Also the spelling of til and the space afer til.
/
QError: nyi
\
avgFares:@[fares;(1+til124208:;fares[1+til124209-1]);]
/
QError: til124209
\

/△
show avgFares:avg fares
fares:@[fares;(2*til 5);:;avgFares]
fares
/
9.434272 4.9 9.434272 4.5 9.434272 15.3 9.434272 8.5 9.434272 11.3 22.5 6.9 1..
9.434272
\

/o
@[fares;where null fares;:;avg fares] //If I awant to replace og list, use "`fares"
```
## 4.5 Dictionaries(99h)

[Dictionaries](https://code.kx.com/q/basics/dictsandtables/) are first-class objects in q. (They are known as _hashmaps_ in some other languages.)

Use the [Dict operator `!`](https://code.kx.com/q/ref/dict/) to make a dictionary from a list of keys and a list of values.

```q
d:`a`b!0 1
d
/
a| 0
b| 1
\

d[`a]
d[`a]:2
d
/
0
a| 2
b| 1
\

d
d1
`0
d+d1 // add values for common keys
d1,d /Right-first merging
// catenation - updates values for common keys, inserts new keys. Typical application is updating a snapshot with deltas.

/
a| 2
b| 1
c| 3
a| 5
b| 6
c| 7
d| 8
0
a| 7
b| 7
c| 10
d| 8
a| 2
b| 1
c| 3
d| 8
\

```

## 4.6🌟Tables (98h)

```q
// 1. Creating a table from a list of like dictionaries
(`a`b!0 1;`a`b!2 3)
/
a b
---
0 1
2 3
\

// 2. Creating a table with table notation
([]a:0 2;b:1 3)

/
a b
---
0 1
2 3
\

// ? 3. Creating a table from a column dictionary. A table is a transpose (flip) of a conforming dictionary (key of symbols, value of list of equal length lists).
/ filip is the function to change dict to table
flip `a`b!(0 2;1 3) 
/
a b
---
0 1
2 3
\

//([]a:0 2;b:1 3)+([]a:4 5;b:6 7)
([]a:0 2;b:1 3)+([]a:4 5;b:6 7)
/
a b 
----
4 7 
7 10
\


//Tables can be keyed. Here are two of the ways to create a keyed table.

//1.Specify key columns with the xkey keyword
k:`a xkey ([]a:0 2;b:1 3)
k
/
a| b
-| -
0| 1
2| 3
\

//2. Specify key columns in the table notation.
([a:0 2]b:1 3)
([a:0 2;b:1 3]c:4 5)
/
a| b
-| -
0| 1
2| 3
a b| c
---| -
0 1| 4
2 3| 5
\

/ key for key value and value for the rest
key abc
`000
value abc
/
a b
---
0 1
2 3
c
-
4
5
\

//A keyed table is a dictionary where both key and values are tables:
key[k]!value k
/
a| b
-| -
0| 1
2| 3
\

k([]a:0 1 2)
`000
([]a:0 1 2)#k
`000
k
/
b
-
1
 
3
000
a| b
-| -
0| 1
1|  
2| 3
000
a| b
-| -
0| 1
2| 3
\
```

### 🌟Exercise
### a. Create a dictionary with keys, `a`, `b`, and `c`, and assign to each key a list of three random ints.
```q
/x
show dic:`a`b`c!0 1 2;`a`b`c!2 3 4;`a`b`c!4 5 6
//it should have been
	dict:`a`b`c!(1 2 3; 10 20 30; 100 200 300)
/
a| 4
b| 5
c| 6
a| 0
b| 1
c| 2
\
dict:[`a`b`c](?10i;?10i;?10i)


/o
dict:`a`b`c!(3?10i;3?10i;3?10i)
dict
//i for int type, ! for dict
/
a| 9 1 2
b| 9 5 7
c| 2 0 2
\
```

### b. Add a new key, `d` with double the values of key `a`.
```q
/x
dict:`d(2*dict[0] 2*dict[1] 2*dict[2])
/
type error
\
dict [`d] 2*[`a]
/
type error
\

/o
dict[`d]:2*dict[`a]
```

### c. Make a table from dictionary
```q
tab:flip dict
tab
/
a b c d 
--------
1 2 9 2 
5 0 0 10
7 2 9 14
\
```

### d. Make a new table by joining the table to itself
```q
/x
([]a:1 2 3 4) + (a[])
/error a

/o
tab
tab2:tab,tab
tab2
/
a b c d 
--------
1 2 9 2 
5 0 0 10
7 2 9 14
a b c d 
--------
1 2 9 2 
5 0 0 10
7 2 9 14
1 2 9 2 
5 0 0 10
7 2 9 14
\
```

### e. Make column `b` the key of this new table
```q
/o
t: `b xkey tab2
t
/
b| a c d 
-| ------
2| 1 9 2 
0| 5 0 10
2| 7 9 14
2| 1 9 2 
0| 5 0 10
2| 7 9 14
\
```

### f. Compare the types of all the generated tables and dictionaries.
```q
/△
type dict
type tab
type tab2
type t

/o
type each (dict;tab;tab2;t)
/
dict| 99
tab | 98
tab2| 98
tabk| 99
\
```

