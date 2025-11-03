
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


