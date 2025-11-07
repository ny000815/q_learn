fox" "the" "quick" "brown" "fox" "the" "quick" "brown"..
,"t"
"qu"
"bro"
"foxf"
\

'[#][1 2 3 4;H]  //funcitonal notation - ' is operating on # to modify it's behaviour
/
,"t"
"qu"
"bro"
"foxf"
\

#'[1 2 3 4;H]    //another funtional form (without being "funcitonal" about the arugment to ')
/
,"t"
"qu"
"bro"
"foxf"
\
```

\'
```q
3 in'(1;1 4;2 3;3 5 6)
3 3 3 3 in'(1;1 4;2 3;3 5 6)  //equivalent
/
0011b
0011b
\

3 1 2 4 in '(1;1 4;2 3;3 5 6) /3 in 1, 1 in 1 4, ...
/0110b
3 1 2 4 in (1;1 4;2 3;3 5 6) / only checking 1. -> 3 1 2 4 in 1
/0100b

a: ("ni ff";1 2 3) ,' ("o tsalB";4 5)
show a: reverse each a
/
"Blast off in"
5 4 3 2 1
\
```

each right(left) /: \\: 
```q
⭐️
/extending by right(left) each
3 in'(1;1 4;2 3;3 5 6)         //previously 
/0010b
3 4 in/:(1# Iterators
- What are iterators?
- Mapping iterators
- Accumulating iterators
- Where to learn more
⭐️ Projection
```q
3 in each (1; 1 2 ; 3 4 )  //this doesn't work - how would you write this functionally?
/error
isThreeIn:3 in        // create a projection of in, making a monadic function that can now be used with each
isThreeIn
/in[3]

isThreeIn 2 3 4 5
/1b
isThreeIn each(1;1 4;2 3;3 5 6)
/0011b

/5ver.
isThereFive:5 within
isThereFive each (3 6; 4 8; 10 15)
/110b

/without func ver
within[5] each (3 6; 4 8; 10 15)
/110b
in[3] each (3 6; 4 8; 10 15)
/100b

```
\'
```q
H:("the";"quick";"brown";"fox") 
1 2 3 4#H 
1 2 3 4#'H       //infix - pairing the operation # to be applied between corresponding values in H and (1 2 3 4) 
/
"the" "quick" "brown" ";1 4;2 3;3 5 6)      //extended to check for both 3 and 4  
/
00b
00b
10b
10b
\

L:("kdb+ is the fastest time-series database";"kdb+/q  has a lambda architecture";"q is the programming language") 
L like\: "*kdb+*" 
/110b

//defining orderIDs and patterns we are searching for 
ids: ("A123";"A234";"B123";"B234")
patterns:("A*";"*123") 
//looking for our patterns 
patternCheck: ids like/: patterns    //we get booleans indicating for each id if they match the pattern
patternCheck
/
1100b
1010b
\
❤️
any patternCheck                          //using "any" we can check if the ids match with any of the patterns
/1110b
ids where any patternCheck                //putting it together using "where" we can retrieve the matching IDs!
/
"A123"
"A234"
"B123"
\
```

prior \': 
```q
/Becareful with the order
0 -': 2 1 2 3 60 62 62
-':[2 1 2 3 60 62 62] //same result as above but different syntax
deltas 2 1 2 3 60 62 62 //same result as above but another different syntax
/
2 -1 1 1 57 2 0
2 -1 1 1 57 2 0
2 -1 1 1 57 2 0
\
-':[200 300 100 500 400 200 100 -200 400 -100]
deltas 200 300 100 500 400 200 100 -200 400 -100 
```

# Ex

roll with each left
```q
rockList:(`i`love`rockandroll)
numerialList:(1 4 3 2)
numerialList?\:rockList
/
,`love
`love`i`rockandroll`love
`love`love`love
`rockandroll`rockandroll
\

?[;rockList]:/numerialList //why this doensn't work..
/ 2
```

`cor`
```

```
---
# PG
reimplementation by using loops of iterators
```q
m:(1 2 3;3 4 5;4 5 6)   / three lists
{i:0;a:();while[i<count x;a,:enlist any 2 3 in x[i];i+:1];a} m
/110b 
any each 2 3 in/: m
/110b
```
???
Pascal's triangle
```q
7 {(+)prior x,0}\ 1
/
1
1 1
1 2 1
1 3 3 1
1 4 6 4 1
1 5 10 10 5 1
1 6 15 20 15 6 1
1 7 21 35 35 21 7 1
\
```

