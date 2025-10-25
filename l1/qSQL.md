## 1. q SQL
### 1.1 Data exploration

```q
tables[ ]
/`smalltrips`trips`weather

trips
/
date       month   vendor pickup_time                   dropoff_time         ..
-----------------------------------------------------------------------------..
2009.01.01 2009.01 CMT    2009.01.01D00:00:00.000000000 2009.01.01D00:04:12.0..
2009.01.01 2009.01 CMT    2009.01.01D00:00:00.000000000 2009.01.01D00:05:03.0..
\

count trips
cols trips
meta trips
\
41859906
c           | t f a
------------| -----
date        | d    
month       | m    
vendor      | s    
pickup_time | p   s

`date`date`month`vendor`pickup_time`dropoff_time`duration`passengers`distance..
\
```

[data types doc](https://code.kx.com/q/ref/#datayeps)

advanced concepts for future

- foreign key
- attribute

---

## 2. qSQL
### 2.1 Basics

```q
select date, month, vendor, passengers, fare, tip, fare+tip from trips 
    where date = min date, tip > 20   

/
date       month   vendor passengers fare tip   tip1  
------------------------------------------------------
2009.01.01 2009.01 DDS    1          33.3 33.8  67.1  
2009.01.01 2009.01 CMT    2          77.4 30    107.4 
2009.01.01 2009.01 CMT    1          47   95.45 142.45
\

/if "fare+tip" was "tip + fare", tip1 would be fare1
```

#### Structure: columnar database
|-2020.01.01 - trips

   |-a.txt

   |-b.txt

|-2020.01.02 - trips

   |-c.txt

   |-.....


-> 1st query runs faster
- 1. `select date, month, vendor, passengers, fare, tip from trips where date = min date, tip > 20`
- 2. `select date, month, vendor, passengers, fare, tip from trips where tip > 20, date = min date`

usage of `i`
```q
select count i from trips where date = min date, passengers = 2

/
x    
-----
78198
\
```
- [qSQL syntax](https://code.kx.com/q/basics/qsql/)
- [_Q for Mortals_ §9: Queries](https://code.kx.com/q4m3/9_Queries_q-sql/)

Exercise 1
`On the earliest date, how many trips had fewer than two passengers?`
<details>
  <summary>Answer</summary>

<pre><code>/x
select i from trips where date = min date
/o
select count i from trips where date = min date, passengers < 2
</code></pre>

</details>


---

### 2.2 Assignment

Create a new variable as follows:

var_name: expression
```q
// Use keyword 'within' to filter the date
jan09:select from trips where date within 2009.01.10 2009.01.31

// Check how many records are in the filtered table
count jan09
`
select count i from trips where date within 2009.1.10 2009.01.31

/
10420159

x       
--------
10420159
\
```


By making this assignment, the new variable jan09 is stored in memory.

This is true whether the source data was also in memory or stored on disk.

Exercise 2
`Assign the result of below to a variable called res2`

`Select payment_type and fare from the trips data for the first date.`
<details>
  <summary>Answer</summary>

<pre><code>/o
show res2: select payment_type, fare from trips where date = min date
</code></pre>

</details>

---

### 2.3  Aggregations 
`sum` is one of many built-in aggregations. Other built-in aggregations include, but are not limited to

- avg - average (mean)
- med - median
- min - minimum value
- max - maximum value
- count - number of values

Reference: [Mathematics and statistics](https://code.kx.com/q/basics/math/)

Exercise 3
`Calculate the minimum and maximum tip from the `jan09` table.`
<details>
  <summary>Answer</summary>

<pre><code>/o
select maxtip: max tip, minTip: min tip from jan09
/
tip tip1
--------
100 0
\
</code></pre>

</details>

### 2.4 Grouping with `by`

```q
select fare by vendor from jan09
/
vendor| fare                                                                 ..
------| ---------------------------------------------------------------------..
CMT   | 6.2 12.6 7 6.6 11 12.2 6.6 14.2 7 9 13 7.4 10.6 10.2 15.8 40.2 9.4 5...
DDS   | 10.9 8.9 9.7 10.9 10.1 6.1 10.9 7.7 8.5 12.1 18.9 10.5 18.5 11.3 9.7 ..
VTS   | 11.3 15.7 18.1 15.7 6.5 4.5 4.5 15.3 4.9 4.9 4.5 6.9 18.5 20.1 14.1 6..
\

select sum fare, sum tip by vendor from jan09
/
vendor| fare         tip     
------| ---------------------
CMT   | 4.430574e+07 2059982 
DDS   | 6120686      273323.2
VTS   | 4.798299e+07 2672231
\

// Get the number of records per day in the filtered table
select count i by date from jan09
/
date      | x     
----------| ------
2009.01.10| 483350
2009.01.11| 405075
..
\
```

Exercise 4
`What is the biggest tip for each vendor?`
<details>
  <summary>Answer</summary>

<pre><code>/o
select maxTip: max tip by vendor from jan09
</code></pre>

</details>

`What is the highest tip and average tip per payment_type?`
<details>
  <summary>Answer</summary>

<pre><code>/x
select maxTip: max tip, avgTip: avg tip by payment_type
/o
select maxTip: max tip, avgTip: avg tip by payment_type from jan09
</code></pre>

</details>


