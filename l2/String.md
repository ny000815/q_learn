
# String Manipulation
# 1.  Learning Objective
- How to create and print strings
- How to use strings for logging
- Common string parsing functions
- Regex string comparison
- Searching strings

# 3. Creating and printing strings
Displaying & Repressing
```q
"Hello" 
"Farewell";   // repressing output 
"World"
1 2 3
2 3 4;
/
Hello
World
1 2 3
\


//0N! & 1 & 2 for stdout
0N!"It's about time";  // what would we see if we remove the ; ?
//"It's about time"

0N!"It's about time"
/
It's about time
"It's about time"
\

-1"Hello World"; a:`other`stuff;
//Hello World
-1"Hello World"
//Hello World

-2 "Errors in Jupyter are indicated by a red band";2+2  //NB execution isn't stopped by writing to stderr
/
4
Errors in Jupyter are indicated by a red band(in error
\
```

\n
```q
//-1 writes to standard out with a carraige return (\n) meaning the next comment will be on the new line

x:0 
while[x<3; 
    1 "Starting with parameter ",string[x];
    -1 " ... Finished";             
    x:x+1;              //incrementing x
    ]
/
Starting with parameter 0 ... Finished
Starting with parameter 1 ... Finished
Starting with parameter 2 ... Finished
\
x:0 
while[x<3; 
    -1 "Starting with parameter ",string[x];
    -1 " ... Finished";             
    x:x+1;              //incrementing x
    ]
/
Starting with parameter 0
 ... Finished
Starting with parameter 1
 ... Finished
Starting with parameter 2
 ... Finished
\
```

# 4. String Manipulation

**Vectors from scalar `vs` and Scalars from vectors `sv`**
```q
vs[";";"a=10;b=20;c=IBM"]    //split up the string on the “;” character 
";" vs "a=10;b=20;c=IBM"     //equivalent to the above
/
"a=10"
"b=20"
"c=IBM"
"a=10"
"b=20"
"c=IBM"
\

show a:vs[";*";"a=10;*b=20;*c=IBM"] //spliting up the string on the string “;*”
vs["="] each a                      //splitting the substrings by "="]
/
,"a" "10" 
,"b" "20" 
,"c" "IBM"
"a=10"
"b=20"
"c=IBM"
\

show Sym: ("AAPL";"TD12kdi12";"34.21") /⭐️ wrapping　with brackets for ""
sv ["|";Sym]
/
AAPL|TD12kdi12|34.21
"AAPL"
"TD12kdi12"
"34.21"
\
```

Padding 
```q
show a:ltrim "             abc   "
show b:rtrim "  abc              "
show c:trim  "         abc       "
"abc"~a
"abc"~b
"abc"~c
/
0b
0b
1b
"abc   "
"  abc"
"abc"
\

10$"example"
-10$"example"
5$"example"
-5$"example"
/
example   
   example
examp
ample
\
```
casting
```q
`small~lower `SMALL
"BIG"~upper "big"
upper " KDB is Fun ! " 
/
1b
1b
KDB IS FUN !
\
```

ith winners from 2010 -> 2029 
/
1b
1b
1b
\

show z:`$y                         //cast the string y to be an atomic symbol `IBM.OQ 
like[z;"IBM*"]                     //like works as before 
`IBM.OQ like "IBM*"
/
1b
1b
`IBM.OQ
\

/To output from list
// write your code here
show L:`JPM.AB`JPM.CD`BM.QW`MSFT.AB
L like "*JPM*"
where L like "*JPM*"
L [where L like "*JPM*"]
/
1100b
0 1
`JPM.AB`JPM.CD
`JPM.AB`JPM.CD`BM.QW`MSFT.AB
\
```

String Search, ss & String Search Replace, ssr
```q
s:"toronto ontario"
count s
s ss "ont"             //pattern occurs at indexes 3 and 8 
/3 8

s ss "[ir]o"           //mix with regex search 
/2 13

s ss "t?r"             //match with tor and tar (? can be any character)
/0 10

show s:"toronto ontario"
ssr[s;"ont";"x"]           
time & memory efficient `like`
```q
\ts:50000  all "tin" = "man"    //we need to add all here, since they only match if ALL characters match
\ts:50000 "tin"~"man"           //we need to have an EXACT match
\ts:50000 "tin" like "man"      //your best friend when comparisng strings is like!
/
16 1264
5 1312
5 1152
\

/with wildcard & other REGEX(Regular Expression)
y:"IBM.OQ"               //A simple string
like[y;"IBM*"]           //Does the input string begin in “IBM” - followed by anything
y like "IBM*"            //infix notation
/
1b
1b
\

like[y;"I?M*"]                       //the “?” is a single wild card character
like["this";"[tT]his"]               //the 1st character can either be “t” or “T”
like["Winners2020";"W*20[12][0-9]"]  //will match w                         / replace "ont" by "x"
/
torxo xario
"toronto ontario"
\
```
### ⭐️Ex
` remove the "o"'s from the string s defined above, only where they are followed by an "n"?`
```q
s
ssr[s;"on";"n"]
/
toronto ontario
tornto ntario
\
//other way
ssr[s;"on";1_] //1_ is dropping first letter, which is "o" on the 1st param
```

# Exam
```q
/making it string
string .z.d + 1
/2025.11.04

/null func to find the place of space
k:"kdb is fun"
where null k
/3 8 11
where not null k
/0 1 2 4 5 6 7 9 10 12 13 14

/Using the string `"kdb plus is fun"`, return a 4 item list of strings: `dList:("kdb";"plus";"is";"fun")`
show dList:vs[" ";k]

//REGEX 0-10
"hogehoge42" like "*[0-9][10]*"

```

❤️⭐️Given the list of strings dList, create the string “Kdb Plus Is Fun” – note the first letters in the list are lowercase but in the result are uppercase.
Can you ALSO do this using the string "kdb_plus_is_fun"?
```q
/utilization off [;0]
dList[;0]:upper dList[;0]  //updating the first index of each list
" "sv dList            //joining each string with spaces between

//other way  utilizing "," join and the index of _
show a:"kdb_plus_is_fun"
show inds: 0,1+ where a="_"  //first index and then the first character after spaces
show a:@[a;inds;upper]
show ssr[a;"_";" "]

//all on one line together
show a:"kdb_plus_is_fun"
ssr[;"_";" "] @[a;0,1+where a="_";upper]


```

