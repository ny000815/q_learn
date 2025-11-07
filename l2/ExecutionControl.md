# Execution Control
### Points
- Signaling Errors
- The conditional `if`
- Atomic conditional evaluation
- Vector conditional evaluation
- Protected evaluation
- Other useful control statements

\Lambda func defining and calling
```q
{[] a:10; b:3; a+b }[] 
13
```

if statement
```q
.z.t
x:10;
if[.z.t > 05:00:00 ;1"Evaluating within the if ... "; 
        x:x+1; //incrementing x 
        y:10; //defining new variable y 
        -1"Complete";
        y]    //note there is no value returned from an if!

x //checking if x is incrementing 
```
by force return (it only works in function)
```q
x:10;
{[]if[.z.t > 05:00:00 ;1"Evaluating within the if ... "; 
        :.z.t; //return the time - subsequent statements not evaluated 
        x+:1; //incrementing x 
        y:10; //defining new variable y 
        -1"Complete";
        y]}[]
x  // not updated as other expression not evaluated after the force return
/
05:21:25.119
10
Evaluating within the if ...
\
```

```q
var1:`a
if[not -19h=type var1;
    -2 "Error: Variable var1 input not of type: 19h";  //writing to stderr
    /exit 1]   //within a script, we might exit early after throwing the error
    /'"Inappropriate value passed for var1 - expected input time, got type:", string type var1;  //custom error
    -1 "Variable var1 not of the right type, defaulting to the current time:", string t:.z.t;
    var1:t]
var1
```

conditional statement mistake
```q
/x
type x = -11h
/o
-11h = type x
```

