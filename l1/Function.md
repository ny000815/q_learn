# 5. Functions


### 5.1 Function Usage
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
res:{neg (y*(1+x) xexp 2) %-1+2*1+x}
res[10;5]
/
-28.5
\
```

