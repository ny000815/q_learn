# 2. Lists
infix notatiion & functional notation
```q
⭐️
add:+      // create your own function add from primitive addition operator
add4:4+    // project the + operator (could also be +[4] or +[;4])
//i.e. isPositive:>0 doesn't work because 
⭐️
isPositive:>[;0]
//This actually worrks
isPositive:0<
```

drop and the order of the q interpretation
Create `list4`, which is the first 2 elements of `list1` joined to everything except the first element of `list2`. 
```q
⭐️
show list4:(2#list1),1_list2
/x
show list4:2#list1,1_list2
``` 

single element list
```q
show a:enlist 2f
⭐️
show a:(),2.0  
```
mix list(list of lists)
```q
show list5:(list1;list2;list3) /without(), interpreter wouud do list5:list1 & list2 & list3 3 different command
/
0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19
13 10 20 38 37 24 15 31 17 27 16 31 33 39 33 33 11 15 25 12 28 36 27 13 11 29..
0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 13 10 20 38 37 24 15 31 17 ..
\
/x
show list5: list1,list2,list3
/0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 13 10 20 38 37 24 15 31 17 ..
```
3 ways to use index
```q
show thirdList5:list5[2]
list5 2
⭐️
@[list5;2]
```
indexing by 2 nums
```q
show thirdSecondList5:list5[1][2]
list5[1;2]
⭐️
.[list5;(1;2)] //. for depth not @
```
all of 2nd item
```q
⭐️
show secondEachList5:list5[;1]
show secondEachList5:.[list5;(::;1)]
```
count of >5
```q
sum list3 > 5
```
indexing can be condition
```q
list3[where list3 > 5]:0Nj
@[list3;where list3 >4;:;0N] //[x;i;f;y]
```
- [`except`](https://code.kx.com/q/ref/except/)
- [`in`](https://code.kx.com/q/ref/in/)
Find the indexes of list6 which are in the intersection of list6 and list7.
```q
/o
list6 in list7
/00011b
where list6 in list7
/3 4

/x
list6
/13 13 7 16 1h
list6 inter list7
/16 1h
list6 in list6 inter list7
/00011b
```
- [`union`](https://code.kx.com/q/ref/union/)
- [`distinct`](https://code.kx.com/q/ref/distinct/)
- [`inter`](https://code.kx.com/q/ref/inter/)
```q
1 3 inter 1 2 4 1
/,1
1 2 4 1 inter 1 3
/1 1
```


