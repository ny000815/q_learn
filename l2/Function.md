# Function

-5
```q
.Q.gc
```

rank error
happens if there's too many argument

force return, : & stdout -1
```q
//Example 3  - force return before print
convertToCelcius:{[farenheit] :offset:farenheit-32; //using : to force return   
                            -1 "Finished offset, starting ratio conversion";
                            offset*5%9;              
                    }                               
convertToCelcius[70]   //returns just offset value before print
/38

cappedDivide:{[numerator;denominator] 
                    if[denominator=0; :20f];  //if[1b; do these things], if[0b;they don't get done]
                    v:numerator%denominator} 
cappedDivide[100;20]
cappedDivide[100;0]
/
5f
20f
\

/ Can be rewrite below because 100%0 would be 0w(infinite) but this is capping with 20
cappedDivide:{[numerator;denominator] 
                   min(20f;numerator%denominator)} 
cappedDivide[100;3]
cappedDivide[100;20]
cappedDivide[100;0]
/
20f
5f
20f
\
```

User defined function cannot take infix notation
```q
divide:{[numerator;denominator] numerator%denominator} 

divide[3;5]    //functionally calling
divide[3][5]
divide[3] 5 
divide[;5] 3
divide[;5][3]
3 divide 5     //infix calling throws an error
```

Usage of `?`
```q
3?5
// this produces 3numbers from "0" - "4" NOT 1 - 5. 
/Omake
til 11
// this produces numbers from 0 to 11
```

Global Variable
```q
b:4
f:{b*x}          //calling b but we haven't defined it locally 
f[1]
delete b from `. //removing b from the global scope 
f[1]             //b now undefined 
/
4
.
error: b
\
delete b, c from `.  //clearing b and c from global scope 
{`b set 47;c::74;}[] // We can see both b & c are defined outside the function after execution
b
c
/
.
47
74
\
```


::
If it is set by :: in func, it can be accessed by outside of the scope

# Ex
func isWholeNumber
```q
isWholeNumber:{type[x] in -5 -6 -7h}
isWholeNumber[4h]
isWholeNumber[2.1]
/1b
/0b
```

[flip](https://code.kx.com/q/ref/flip/)
```q
q)flip (1 2 3;4 5 6)
1 4
2 5
3 6
```
[cut](https://code.kx.com/q/ref/cut/)
```

```

[?(find)](https://code.kx.com/q/ref/find/)
```q

```

$(casting)
```q

```

func string can take multiple inputs

[#]()

[\`vs \`:/aaa/aaa.txt] & hsym
```q

```

