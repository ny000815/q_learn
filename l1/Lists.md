# 4. Lists

## 4.1 Data types
h is suffix for indicating data types
```q
vtsfares:select fare from trips where date = 2009.01.01, vendor=`VTS
vtsfares
type vtsfares
/
fare
----
5.7 
4.9 
4.9 
..
98h
\

fares: vtsfares`fare
fares
type fares
/
5.7 4.9 4.9 4.5 4.9 15.3 3.7 8.5 13.3 11.3 22.5 6.9 16.5 24.1 9.3 9.3 6.1 8.1..
9h
\

//While simple lists always have strictly positive values returned by type, general lists always have type zero.
general:(`VTS;23.45);
general
type general
/
`VTS
23.45
0h
\

//A list can be _empty_ - if there had been a typo in the select statement, and we inquired about a non-existent cab company, we would see:

//if it is negative, it is scalar type
aaabbbccc: 9.93
type aaabbbccc
/
-9h
\


```

