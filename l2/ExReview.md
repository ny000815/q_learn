# 1.Atom
infix notation & functional notation
```q
⭐️
add:+      // create your own function add from primitive addition operator
add4:4+    // project the + operator (could also be +[4] or +[;4])
//i.e. isPositive:>0 doesn't work because 
⭐️
isPositive:>[;0]
//This actually worrks
isPositive:0<
```
# 2. Lists
drop and the order of the q interpretation
Create `list4`, which is the first 2 elements of `list1` joined to everything except the first element of `list2`. 
```q
⭐️
show list4:(2#list1),1_list2
/x
show list4:2#list1,1_list2
``` 

single element list
```q
show a:enlist 2f
⭐️
show a:(),2.0  
```
mix list(list of lists)
```q
show list5:(list1;list2;list3) /without(), interpreter wouud do list5:list1 & list2 & list3 3 different command
/
0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19
13 10 20 38 37 24 15 31 17 27 16 31 33 39 33 33 11 15 25 12 28 36 27 13 11 29..
0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 13 10 20 38 37 24 15 31 17 ..
\
/x
show list5: list1,list2,list3
/0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 13 10 20 38 37 24 15 31 17 ..
```
3 ways to use index
```q
show thirdList5:list5[2]
list5 2
⭐️
@[list5;2]
```
indexing by 2 nums
```q
show thirdSecondList5:list5[1][2]
list5[1;2]
⭐️
.[list5;(1;2)] //. for depth not @
```
all of 2nd item
```q
⭐️
show secondEachList5:list5[;1]
show secondEachList5:.[list5;(::;1)]
```
count of >5
```q
sum list3 > 5
```
indexing can be condition
```q
list3[where list3 > 5]:0Nj
@[list3;where list3 >4;:;0N] //[x;i;f;y]
```
- [`except`](https://code.kx.com/q/ref/except/)
- [`in`](https://code.kx.com/q/ref/in/)
Find the indexes of list6 which are in the intersection of list6 and list7.
```q
/o
list6 in list7
/00011b
where list6 in list7
/3 4

/x
list6
/13 13 7 16 1h
list6 inter list7
/16 1h
list6 in list6 inter list7
/00011b
```
- [`union`](https://code.kx.com/q/ref/union/)
- [`distinct`](https://code.kx.com/q/ref/distinct/)
- [`inter`](https://code.kx.com/q/ref/inter/)
```q
1 3 inter 1 2 4 1
/,1
1 2 4 1 inter 1 3
/1 1
```


# list rev  

```q
list6 where not list6 in list7  
list6 except list7  
```


# 3.String  
>cast to atom?  

- ss -> return index  
- "hello" = "o" -> return bool  
- null can be used to find " " -> return bool  
- boolean list can be inverted by putting not at first  
- need of first pbc for vs where to split  
- editing the first letter of each word in a string that represents a list of lists.  
```q  
dist[i;0]:upper dist[i;0]  
```

```q  
/both o  
a[i]: upper a[i]  
a[i]:@[a;i;upper]
```
- trim micro trim  
- = applies to each item in the list and return like 1 0 0 1 b, while "~" returns 1b only when all items.  
- Order matters for "like" which accepts Regex.  
```q
"abcd" like "abc*"  

/x  
"abc*"~"abcd"  
```

- except and ,QnA  
- ssf with each  

```q  
ss[i;"s";"o"] each s  
```

- using drop`_` for extracting the path -> hsym(host symbol)

```q
"/mount/folder/file.txt" =>  
../mount/folder/file.txt
```


# 4.Cast  
`uu$.z.t
`ss$.z.t
`hh$.z.t
- converting time to timestamp +.z.d  
- Use capital letter to cast from strings : "T"$12:00:00"  
- Casting to symbol : \`$ "abc; bbc; cba" ->why? other way?  


# 5.Func  
- Don't forget ';' at the end of lines of function  
- Usage of 'in' and list for multiple choice of type : (type x) in -5 -6 -7h  
-  `@` Cleaner way
```q
doubleSecondItem:{@[x;1;:2*x[1]]}
->
doubleSecondItem:{@[x;1;2*]}
```
- `?` usage and the order. Opposite for `in` and `like`
```q
3 4 6?6
/2
```
string with multiple args
```q
toFullName:{`$" " sv string (x;y)}

/faster than string a,b,c
x:`a
y:`b

\ts string (x;y) 10
/0 1040

\ts string (x,y) 10
/01152
```
❓❓
hsym(host shmbol)
Create a monadic function `parsePath` that takes a full q filepath (as a string) [":/mount/folder/file.txt"] and returns the directory and file (as strings)
Example of Code:
```q
parsePath:{string ` vs hsym`$x}
parsePath[":/mount/folder/file.txt"]
/
":/mount/folder"
"file.txt"
\
```

# 6 Iterator
- \`
```q
x: 10 30 20 40 
y: 13 34 25 46
x, ' y
/
  10 13 
  30 34 
  20 25
  40 46
\

/x
each x , each y
/error
```
- each left each right, the order  `x ,\: y`
- both each usage (with bracket)
```q
1 -2 _' (x;y)
```
- roll, ? with list
```q
1 2 5 ?\: `ab`bc`cd
/
,`bc
`ab`cd
`bc`cd`bc`ab`bc
`i`love`rockandroll
\
```
- drop_ usage, dropping one letter a \_ num

# 7. Execution Control
- `rand` if asked to gen random number, need to clarify the type, atom or list
```q
/the difference of return result by "<" ">" "=". Only later one works for if's condition
show v: 1?100
v > 50
/,1b
/51
show a: rand 100
v > 50
/1b
/51

if[v>50; v*:2]
/ error type

if[a>50; v*:2]
/ 102
```
- ❓is 1st one better??
```q
'`$"abc"
/or
-2"`abc"
```
- if-else with `$`
```q
Pasting this data from Prisma Access Browser is prohibited
```
- non conditional expression
```q
OddOrEven:{`even`odd x mod 2}


isFloat:{9h = abs type x}
```
- else if
```q
abc:{$[x~`a;1;x~`b;2;x~`b;3;0]}
```
- `?` for vector conditional evaluation
```q
replaceNegatives:{?[x<0;0;x]}
/which one has better performance?

replaceNegatives:{@[x;where x < 0;:;0]} 
//Thouhgt the 2nd last arg can be ":0".but it makes whole list 0. It can be *2 or upper
```
- (%2 vs \*0.5 Performance comparison)
```q
\ts 353943280%2 100
/0 784
\ts 353943280*0.5 100
/0 784
```
- ⭐❓ giving the arg to inner func
```q
add1:{[a]{[a]a+1}[a]}
{[a]a+1}[a]
```
- ⭐calling func with protection
```q
/@[f; x; e]
@[add1;`a;0]
@[add1;`a;{show x;0}] // showing error type

⭐　/.[g; (a;b;...); e]   list's list ver.g≒f
/implementing inside the func
multiply:{.[*;(x;y);{"Error: The inputs should be numerical value"}]}
```

- ⭐fizzbuzz
```q
fizzbuzz:{
    {$[0 = x mod 3;
        $[0 = x mod 5;
            "fizzbuzz";
            "fizz"];
        0 = x mod 5;
        "buzz";
        x]} each x}
Input: 1 2 3 4 5 6 7
fizzbuzz[Input]

//inner {} is needed for "each". each format:"f each list". Without it, type error

/
// Create list of arguments
arg:1+ til 100
fizzbuzz: {
    {$[0=x mod 3;
        $[0=x mod 5;
            `fizzbuzz;
            `fizz];
        0=x mod 5;
        `buzz;
        x]} each x}
20 sublist fizzbuzz arg
\
```

- `like`'s output is boolean
- ⭐converToMile
```q
arg:("1 mile";"5 km";"3 miles";"7.5 km";"3 km";"4.2 km";"0.5 miles")
convertToKm:{b:x like "*mile*";        // Create boolean like of those that are miles
    v:"F"$first each " " vs/: x;       //Extract numerical value from string
    ?[b;1.609*v;v]                          // Apply vector conditional
}
convertToKm arg



❓ /where to fix?
convertToKm:{
    mi:where x like "*mile*"
    show num:"F"$first each " " vs/: x
    @[num;mi;*:1.609]
	}
Input: ("1 mile";"5 km";"3 miles";"7.5 km";"3 km";"4.2 km";"0.5 miles")
convertToKm[Input]

/error num 
```
---

# 5. Func

xexp
	2 exp 8 
	256

assign error
	using upper as a vairable which is taken by func

forgetting semicolon

minus and neg difference
	neg would be done at the end so
	-1 +--- vs neg 1 --- would be totally different because of the sign difference

precendence
	type [x] in 5 6 7h

⭐️finding place getIndexOf[`john`mike`pat`tim`harry;`pat] -->2
	where x = y return list.
	x?y returns atom

\* $
	casting to \*. 

symbol vector to string
	making it to string first.
	```
	`$"."sv string `quick`brown`fox
	```

split hsymed path to path and file
	\` vs hsym "aa/bb/cc"


# 6 Iterator

+roll right? left?
	3?5 /left is the num right is numlist or limit
	0 4 0

cor 

# 7 Execution Control
if doesn't have infix notation

using error message(Not warning)
	```q
	'`$"error123"
	```

if else
	$[condition;xpattern;yelsepattern]

if else or vector
	?[condition;xpattern;yelsepattern]

⭐️givin parameter to func right after definition
	```q
	/fixing this
	add1:{[a] {a+1}[a]}
	/2options
	add1:{[a] {x+1}[a]} 
	add1:{[a] {[a]a+1}[a]}
	```

Trapping (Safe Monadic Function Calling)
	@[function;arguments;errorFunction]

Trapping (Safe dyadic/polyadic Function Calling)
	```q
	.[calculation;(x;y;z);{"error: ",x}]
	multiply:{.[*;(x;y);{-2"The inputs should be of numerical types"}]}
	q).[{x+y+z}; (20;50;"c"); {"Function has returned an error: ", x}]
	
