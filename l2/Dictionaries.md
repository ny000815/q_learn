
- `!` bang to define dictionaries
- `?` Reverse Look Up
```q
dict
/
john  | 20
steve | 31
rachel| 22
\
dict?20 /(to look up by key, dict `John or dict[`John])
/john

/Other ? usage
/find
10 20 30 40 ? 20
/1

/roll
3?5
/0 4 2
```
- `#` Take  and setting default value for missing value with `^`
```q
`tom`jane # dict            //selecting just the values for Tom and Jane
/tom | 18
/jane| 21
`timothy`sarah # dict       //selecting values that aren't present assigns a null 
/timothy| 
/sarah  | 31
23^`timothy`sarah # dict    //setting a default of 23 for any missing values
/timothy| 23
/sarah  | 31
```
- `^` Coalesce
```q
d3:`a`b`c`d!2 2 3 5
d4:`c`d`e`f!20 0N 31 5
d3^d4 //rather than keeping the null value of c in d2, the value from d1 is retained
/
a| 2
b| 2
c| 20
d| 5
e| 31
f| 5
\
```

# Ex
- `;` one dic with multiple type -> dictionaries of dictinaries
```q
/write your code
nums:1 2 3!`one`two`three
times:12:00 00:00!`noon`midnight
dates:2020.04.03 2020.04.02!`today`yesterday

show dicts:`nums`times`dates!(nums;times;dates)
/
nums | 1 2 3!`one`two`three
times| 12:00 00:00!`noon`midnight
dates| 2020.04.03 2020.04.02!`today`yesterday
\
```
- ⭐️dictionary mapping
```q
/write your code
index:1 2 3 4 5!("eu";"sa";"us";"as";"")
key[dict]
/`Italy`Spain`Norway`Brazil`United States`Yemen`Mexico`Albania`Japan`Australia
value dict
/1 1 1 2 3 4 2 1 4 5
index value dict
/
"eu"
"eu"
"eu"
"sa"
"us"
"as"
"sa"
"eu"
"as"
""
\
(key dict)!index value dict
/
Italy        | "eu"
Spain        | "eu"
Norway       | "eu"
Brazil       | "sa"
United States| "us"
Yemen        | "as"
Mexico       | "sa"
Albania      | "eu"
Japan        | "as"
Australia    | ""
\
```
- ❤️fizzbuzz with dict
```q
arg: 1+ til 100 
fizzbuzz: {show fizz_inds: 0=x mod 3; 
            show buzz_inds: 2*0=x mod 5;
            show inds: fizz_inds+ buzz_inds ;  //fizzbuzz will be 3 
            show map: 1 2 3!`fizz`buzz`fizzbuzz;
            (`$string x)^map inds    //map the values using the dictionary and then fill nulls with the values 
 } 

fizzbuzz arg
/
`1`2`fizz`4`buzz`fizz`7`8`fizz`buzz`11`fizz`13`14`fizzbuzz`16`17`fizz`19`buzz..
00100100100100100100100100100100100100100100100100100100100100100100100100100..
0 0 0 0 2 0 0 0 0 2 0 0 0 0 2 0 0 0 0 2 0 0 0 0 2 0 0 0 0 2 0 0 0 0 2 0 0 0 0..
0 0 1 0 2 1 0 0 1 2 0 1 0 0 3 0 0 1 0 2 1 0 0 1 2 0 1 0 0 3 0 0 1 0 2 1 0 0 1..
1| fizz
2| buzz
3| fizzbuzz
\
```


