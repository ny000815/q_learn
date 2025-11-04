# Casting
### Points
- Casting as a concept
- Implicit casting
- Using `$` to cast data to different datatypes
- Casting textual data

Time type
12 Time Stamp
14 Date
16 Timespan
18 second

Explicit casting in kdb+/q uses the [`$`](https://code.kx.com/q/ref/cast/) (aka [cast](https://code.kx.com/q/ref/cast/)) operator.
Assuming the data you wish to cast is **not a textual datatype (like symbol or string),** any of the following three approaches can be utilized by `$`to specify the desired type of the output: 1. It's symbol name e.g `float 2. It's character letter e.g. "f" 3. It's short value (aka short name) e.g. 9h

symbol can can be casted only by string
```q
` $ string 3.14159
` $ "3.14159"

`symbol$ "3.14159"
/ error
```


symbol can be casted only to string
```q
//???
7h $ string `0`1`2`3
/
48
49
50
51
\

"J" $ string `0`1`2`3
0 1 2 3

-7h $ string `0`1`2`3
0 1 2 3
```
