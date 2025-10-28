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

## 4.2 Casting
 `$` used to [cast](https://code.kx.com/q/ref/cast/) an empty list.
[List](https://code.kx.com/q4m3/3_Lists/)
```q
1 2
`float$1 2 //using it's symbol name 
"f"$1 2  //using it's character letter
9h$1 2   //using it's short value
`*
type 1 2
type`float$1 2 
type "f"$1 2  //using it's character letter
type 9h$1 2   //using it's short value
/
1 2
1 2f
1 2f
1 2f
*[`]
7h
9h
9h
9h
\

() //general list 
`long$() //list of type long
/
()
`long$()
\

```

### sublist
```q
fares
/
5.7 4.9 4.9 4.5 4.9 15.3 3.7 8.5 13.3 11.3 22.5 6.9 16.5 24.1 9.3 9.3 6.1 8.1..
\

10 sublist fares
/
5.7 4.9 4.9 4.5 4.9 15.3 3.7 8.5 13.3 11.3
\

-10 sublist fares
/
12.9 4.9 44.5 5.3 19.3 9.3 8.9 45 9.7 4.5
\

//sublist has a feature that the number of elements it returns is _capped_ at the size of the list that it operates on.
count fares
count 10000000 # fares
count 10000000 sublist fares

/
124208
10000000
124208
\



```

### Exercise 9
Use sublist to get the second 10 elements in the list
<details>
  <summary>Answer</summary>

<pre><code>/▲
subA:20 sublist fares
-10 subA subList
/
22.5 6.9 16.5 24.1 9.3 9.3 6.1 8.1 5.7 8.5
\

/o
-10 sublist 20 sublist fares
/
22.5 6.9 16.5 24.1 9.3 9.3 6.1 8.1 5.7 8.5
\
</code></pre>

</details>

### sort
```q
fares
show sortedFares:asc fares
10 sublist sortedFares
-10 sublist sortedFares

/
`s#2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2...
5.7 4.9 4.9 4.5 4.9 15.3 3.7 8.5 13.3 11.3 22.5 6.9 16.5 24.1 9.3 9.3 6.1 8.1..
2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5 2.5
104 104 108.5 112.5 120 122 128 134.3 190 200
\
```


