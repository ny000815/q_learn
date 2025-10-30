# 5. Functions


## 5.1 Function Usage
calling & defining function
```q
max[10 11 12]  // functional notation
max 10 11 12   // infix notation
/
12
12
\

speed:{[miles;hours]
 mph:miles%hours;
 kph:1.609*mph;
 // return the speed in kph
 :kph;
 }
 
 speed[15;0.5]
/
48.27
\
 
//If there is no [explicit return](https://code.kx.com/q/basics/function-notation/#explicit-return) from a function its result is the result of evaluating the last expression in it. So the code above can be rewritten as
speed:{[miles;hours]
 1.609*miles%hours  // NOTE NO SEMICOLON
 }
speed[15;.5]        // result is unchanges
/
48.27
\

speed[15 30 20;.5 .9 1.0] /1.609 * (15/0.5; 30/0.9; 20/1.0)
/
48.27 53.63333 32.18
\
```

### Exercise 
create function, res = -(y(x+1)^2) / (2(x+1)-1)
```q
res:{[x;y]
  a:y+ x+1 xexp 2
  b:-1+2*x+1;
  a%b;
}
/
error {
\

/o
res:{
    a:y*xexp[x+1;2];
    b:-1+2*x+1;
    neg a%b
    }
res[10;5]
/
-28.5
\
```

### Explicit and Implicit parameters
```q
speed:{[miles;hours] //explicit parameters
 1.609*miles%hours 
 }

speed:{1.609*x%y} //implicit parameters
```

### Call functions from qSQL
```q
jan09:select from trips where date within 2009.01.01 2009.01.07
select spd:speed[distance;duration % 0D01:00],distance,duration from jan09 where vendor = `VTS
/
spd      distance duration             
---------------------------------------
48.5918  1.51     0D00:03:00.000000000 
-17.3772 1.26     -0D00:07:00.000000000
14.28792 0.74     0D00:05:00.000000000 
16.8945  0.7      0D00:04:00.000000000
..
\
/ Combined version
select avgspeed:speed[sum distance;sum[duration]%0D01:00] by vendor from jan09


```
We use `duration % 0D01:00` to give us a number of hours as a floating point number from the nanosecond precision duration we have stored in the trips table.
### Exercise 
Write a function `createTable` that selects from `jan09` the columns `vendor`, `distance`, and `tip`; and adds a new column from the result of `tipOverDistance` applied to columns `tip` and `distance`.
```q
/x??
createTable:{
    select  tipPerDistance:tipOverDistance[tip;distance], vendor, distance, tip from jan09;
    }
createTable
/
{
    select  tipPerDistance:tipOverDistance[tip;distance], vendor, distance, t..
    }
```



### Accumulating iterators
```q
N:1 4 7 10                         / numeric list
+/[N]                             / sum      (Over)   
+\[N]                             / sum      (Scan)   
*/[N]                             / product  (Over)
*\[N]                             / products (Scan)
/
22
1 5 12 22
280
1 4 28 280
\

-/[N]
-\[N]
/
-20
1 -3 -10 -20
\

(+/)1 2 3 4 / Add these numbers, fold '+' over the vector; fold is sometimes called reduce or inject
(*/)1 2 3 4 / Extends to all functions in the expected way
sum 1 2 3 4 / Another way to sum the values, using a built-in function
(+\)1 2 3 4 / Cumulative sums, using scan
sums 1 2 3 4 / Same, using the built-in function
/
10
24
10
1 3 6 10
1 3 6 10
\
```

### Exercise
a. Create a new function, add:{x+x}, and iterate this list of integers across it, 3 6 8
```q
add:{x+x}
/x
/(add2/)3 6 8
/
17
\
add each 3 6 8
/
6 12 16
\
```
c. Multiply each value in this list, 3 5 4 2, against the value 11. Use both scan and over.
```q
// Enter your code here 
/(*/) 3 5 4 2
/(*\) 3 5 4 2
multiply:{x*y}
(multiply\)[11;3 5 4 2]
(multiply/)[11;3 5 4 2]
/
33 165 660 1320
1320
\
```

### ⭐️⭐️ Bonus Exercise -Fibonacci
```q
exerFib:{{x, sum -2#x}/[x;0 1]}


x:0 1
sum -2#x
-2#x
2#x
`-
x,sum -2#x       / Beginning of Fibonacci sequence
{x,sum -2#x} 1 1 / Same, but as an (unnamed) function
fib:{x,sum -2#x} / Name it
fib/[10;1 1]     / Similar to +/ example earlier, apply the function repeatedly 10 times
10 fib/ 1 1      / Another way to invoke the function

/
1
0 1
0 1
-[`]
0 1 1
1 1 2
1 1 2 3 5 8 13 21 34 55 89 144
1 1 2 3 5 8 13 21 34 55 89 144
\

exerFib[20]
/
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181 6765 10946
\
```

