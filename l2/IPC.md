# IPC


how to cast timestamp to for xbar 1 hour 
```q
1 xbar time.hh
```
Sending result or Sending func with variable
```q
h"myFunc:{x+y}" /remote
myFunc:{x-y} /local
h (myFunc;3;2)
/1
h (`myFunc;3;2)
/5
```

# IPC
### 1.Open and Check 
 
```q
.z.W
hopen h:5050
.z.W
/
9 | 0  
11| 0  
13| 0  
16| 0  

::  

17i  

9 | 0  
11| 0  
13| 0  
16| 0  
17| 0
\
key .z.W
h in key 
hclose h
h in key 
key .z.W
/
9 11 13 16 17i
1b
::
0b
\
```

### 2.symbol method, string method and functional method
```q
h `quote
/
time sym size ask bid  
--------------------------------------------------  
0D00:00:00.057522049 MSFT 725 12.007 11.41461  
0D00:00:00.261543850 GE 575 5.064666 4.570663
...
\

h "select count i by sym from trade"

```
⭐️ex
Using the handle `h`, send a query to the remote process that will return the average spread between the bid and ask broken down by hour.
```q
h "select spread: avg ask-bid by 1 xbar time.hh from quote"
```
using sym list to retrieve data
```q
symList: `JPM`GE`TSLA`KO 
"`","`" sv string symList
show "select from trade where sym in ","`","`" sv string symList
h "select from trade where sym in ","`","`" sv string symList
/
"JPM`GE`TSLA`KO"  
"`JPM`GE`TSLA`KO"  
::  
"select from trade where sym in `JPM`GE`TSLA`KO"
time sym size price side exchange  
-----------------------------------------------------  
0D00:00:00.606767613 GE 725 11.85308 S L  
0D00:00:04.384579288 JPM 575 4.769245 B L  
...
\
```
functional method
```q
3 sublist t: 10000#([]a:1 2 ; b: 20 30)   //a local table
h (set;`tab;t)     //sending to define remotely using set
3 sublist h "tab"  //retrieving back using the string method
/
a b   
----  
1 20  
2 30  
1 20    

`tab  

a b   
----  
1 20  
2 30  
1 20  
\
```
⭐️Ex
- Define a variable `myVar` of value 12.34 using the string method
- Return `myVar` using the symbol method
- ⭐️Redefine `myVar` to 302.1 using the functional method
```q
h "myVar:12.34"
h `myVar
h (set;`myVar;302.1)
h `myVar
/
::
12.34
`myVar
302.1
\

```
### 3.Passing by Reference versus Value
```q
h"myFunc:{x+y}" /remote
//sending a function for remote execution 
h ({x+y};2;3)     //remote def and run syntax: <handle> (<function>;<param1>;...) 
myFunc:{x-y} /local
h (myFunc;3;2)
/1
h (`myFunc;3;2)
/5
```
