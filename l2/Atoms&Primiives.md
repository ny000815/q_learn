# 1. Atoms
## 1.1 Atoms & Primitives

z.t. //current time -19h
z.d //current date -14h


# 2. Datatypes

type  
5,6,7,8,9,10,11

hijefcs
short
int
long
real
float
char
symbol

# 3. Primitives


type 6%3

floor

in

logic to detect weekend day by using mod and in

difference between = & ~

mavg


6%3      //division with longs will return a float 

2f


floor[2000%140]

1 in 1 2 3 // can be in[1;1 2 3]

1 4 in 1 2 3

1b

10b


(.z.d mod 7) in 0 1 //In kdb+/q the week "starts" on a Saturday.


= value match 

~ exact match 

1 2 3=1 2.0 3   // when dealing with lists, = compares the lists item-wize

1 2 3~1 2.0 3   // even with lists, ~ always returns a single true or false

111b

0b



# Variables & Assignment

`Create pairMovingSum function`

```q
pairMovingSum :{ msum[2]}

//explicit ver.
parMovingSume{[x]
	msum[2;x]
}
⭐️❤️
//without msum ver. utilizing list + list & droping(_)
pairMovingSum:{ x + 0, -1 _ x }

pairMovingSum[1 2 3 2 3 2 1]
//1 3 5 5 5 5 3
```


# Exam
`Calculate (((5-3)*(2+30))+6)/4 using as few characters as possible.`
```q
/x
(6+(2+30)*(5-3))%4
/o
0.25*6+(5-3)*2+30
```

`Given the list  2 4 1 2 3 return a boolean list which is true where the list contains a value of 2, i.e. 10010b`
```q
/x
2 in 2 4 1 2 3
// 1b
/o
2= 2 4 1 2 3
// 10010b
```

func for standard deviation
dev[x]

func for Differences between adjacent list items
deltas

Create func isPositive
Example results
isPositive -4 0 3
001b
```q
isPositive:>[;0] //same as {[x] x>0}
//f[;c] is "projection" to make c as a fixed second papram
/similar examples
minus2:-[;2] //same as minu2{[x] x-2} & minus2:-2+
subtractFrom10:-[10;] 
double:*[;2] //can be double:2*
//
```


