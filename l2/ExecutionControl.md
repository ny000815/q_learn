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

with previous topics
```q
//temperature Actions
tooLow:{-2"Temperature too low - ironing ineffective"; min (0,x)}    //what should happen when too low
tooHigh:{-2"Temperature too hot! Stop Immediately!!"};                //what should happen when too high
justRight:(::)   //function null - do nothing when fine

//temperature Thresholds 
woolThresh: 160 170      //for wool 
viscoseThresh: 150 180   //for viscose 

//core monitor logic
tempMonitor:{[sensorTemp;thresh]   
                function:$[sensorTemp> thresh[1]; 
                        tooHigh;
                    sensorTemp < thresh[0]; 
                        tooLow;
                        justRight]; //since we need a function to call we do need to include here
                 function[sensorTemp]}

//projections for each material
woolMonitor:tempMonitor[;woolThresh];
viscoseMonitor:tempMonitor[;viscoseThresh];
```
type detection[❓]
```q
myFunc:{
  s:$[10h=type y; y; string y];
  $[11h=abs type x; lower s; s]
}
```

ternary operator for lists[❓]
```q
/x
myFunc:{?[x~0N;10;L]}

/o
L:1 2 3 0N 5
myFunc:{?[x~0N;10;L]}
myFunc[L]
L
/
1 2 3 10 5
1 2 3 0N 5
\
```

@, Trap
```q
@[function; arguments; errorFunction]
```
The @ operator (“trap”) applies a function to its arguments — but if an error occurs, it calls the third argument (errorFunction) instead.
1st argument → the function you want to call (e.g. sin)
2nd argument → the argument(s) to that function (e.g. `symbol)
3rd argument → a function that handles the error (receives the error message as input)
```q
argument:`symbol
handleError:{[arg;error]     2 "This broke it:",string[arg];
                            -2 "... type :",string type arg;
                            -2 "With error:",error;
                             0b};

errorFunction:handleError[argument]     //projecting to one argument

@[sin;argument;errorFunction]  
/
0b
This broke it:symbol... type :-11
With error:type
\

protectedSin:{[arg] @[sin;arg;handleError[arg]]}   //use protected evaluation to call sin with an argument
protectedSin[90]
/0.8939967
protectedSin[`break]     //failure return 0b
0b
This broke it:break... type :-11
With error:type
```


The way to repeat `\t`
```q
/by putting :num
\t:10 a*a:til 100000
```

while
```q
r:1 1
x:10
while[x-:1;r,:sum -2#r]  
r
/1 1 2 3 5 8 13 21 34 55 89
```

# Ex
rand 100 vs 1?100
```q
show x:rand 100 / schalar
/3
show x:1?100 / list
/,3
```
if not
```q
tomorrow:{if[not -14h~type x;
                '`$"'Parameter should be a date"];
            x+1}
```

```q
/x
replaceNegatives:{?[x < 0; x:0]}
/o
replaceNegatives:{?[x<0;0;x]}
```

sym data processing
```q
L: `abc3A`abdB`dakB
last each string L
/ABB
```

