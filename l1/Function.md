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


