# 1. q SQL
### Learning objectives

To understand:

- How to construct a qSQL query
- How to assign a variable
- Useful inbuilt aggregations
- Grouping with by
- Using fby to avoid nested queries
- Updating existing data
- Temporal arithmetic

## 1.1 Data exploration

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

# 2. qSQL
## 2.1 Basics

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

### Structure: columnar database
|-2020.01.01 - trips

   |-a.txt

   |-b.txt

|-2020.01.02 - trips

   |-c.txt

   |-.....


-> 1st query runs faster
- 1. `select date, month, vendor, passengers, fare, tip from trips where date = min date, tip > 20`
- 2. `select date, month, vendor, passengers, fare, tip from trips where tip > 20, date = min date`

### Excerpt from [Where phrase](https://code.kx.com/q/basics/qsql/#where-phrase)
```q
q)select from t where c2>15,c3<3.0
c1 c2 c3
---------
b 20 2.2

q)select from t where (c2>15) and c3<3.0
c1 c2 c3 
--------- 
b 20 2.2

/
The examples above return the same result but have different performance characteristics.

In the second example, all `c2` values are compared to 15, and all `c3` values are compared to 3.0. The two result vectors are ANDed together.

In the first example, only `c3` values corresponding to `c2` values greater than 15 are tested.

Efficient Where phrases start with their most stringent tests.
\
```



### usage of `i`
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

### Exercise 1
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

## 2.2 Assignment

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

### Exercise 2
`Assign the result of below to a variable called res2`

`Select payment_type and fare from the trips data for the first date.`
<details>
  <summary>Answer</summary>

<pre><code>/o
show res2: select payment_type, fare from trips where date = min date
</code></pre>

</details>

---

## 2.3  Aggregations 
`sum` is one of many built-in aggregations. Other built-in aggregations include, but are not limited to

- avg - average (mean)
- med - median
- min - minimum value
- max - maximum value
- count - number of values

Reference: [Mathematics and statistics](https://code.kx.com/q/basics/math/)

### Exercise 3
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

### Exercise 4
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
/
payment_type| maxTip avgTip     
------------| ------------------
CASH        | 82     0.00077795 
CREDIT      | 100    2.145682   
Dispute     | 11.25  0.01481096 
No Charge   | 13.35  0.006573717
\
</code></pre>

</details>



## 2.4 Using `fby` to avoid nested queries

```q
// Get the average duration per vendor and save resulting table in a variable
show resBy: select avgDuration:avg duration by vendor from jan09
/
vendor| avgDuration 
------| ------------
CMT   | 6.480017e+11
DDS   | 7.68078e+11 
VTS   | 7.290009e+11
\
// Using 'lj' to join the average duration column to our table
show without Fby select from jan09 lj resBy where duration < avgDuration
/
date       month   vendor pickup_time                   dropoff_time         ..
-----------------------------------------------------------------------------..
2009.01.10 2009.01 VTS    2009.01.10D00:00:00.000000000 2009.01.10D00:08:00.0..
2009.01.10 2009.01 VTS    2009.01.10D00:00:00.000000000 2009.01.10D00:04:00.0..
2009.01.10 2009.01 VTS    2009.01.10D00:00:00.000000000 2009.01.10D00:02:00.0..
..
\
```

In q, this can be simply expressed using [`fby`](https://code.kx.com/q/ref/fby/).
```q
show withFby select from jan09 where duration < (avg;duration) fby vendor
/
date       month   vendor pickup_time                   dropoff_time         ..
-----------------------------------------------------------------------------..
2009.01.10 2009.01 VTS    2009.01.10D00:00:00.000000000 2009.01.10D00:08:00.0..
2009.01.10 2009.01 VTS    2009.01.10D00:00:00.000000000 2009.01.10D00:04:00.0..
2009.01.10 2009.01 VTS    2009.01.10D00:00:00.000000000 2009.01.10D00:02:00.0..
..
\
withoutFby~withFby
/
0b
\
meta withoutFby
meta withFby
/
c           | t f a
------------| -----
date        | d    
month       | m    
..  
tolls       | f    
total       | f    
avgDuration | f    
c           | t f a
------------| -----
date        | d    
month       | m    
..
tolls       | f    
total       | f
\

(delete avgDuration from withoutFby)~withFby

/
1b
\
```


### Exercise 5
⭐️
`Which payment type produces the highest average tip when only trips with a fare larger than the average for each vendor is considered?`
<details>
  <summary>Answer</summary>

<pre><code>/x
select tip by where fby avg payment_type from jan09
select tip where fby (tip > avg tip) by payment_type from jan09
select from jan09 where tip > (avg;tip) fby payment_type
select from jan09 where fare > (avg;fare) fby vendor

/o
res7a:select avg tip by payment_type from jan09 where fare > (avg;fare) fby vendor;
show res7a;
select payment_type from res7a where tip = max tip 
/
payment_type| tip        
------------| -----------
CASH        | 0.001723333
CREDIT      | 3.004373   
Dispute     | 0.02912136 
No Charge   | 0.0112184
payment_type
------------
CREDIT      
\

/? (the position of from affects the column name)(Also show + assignment is done later)
select payment_type where tip = max tip from res7a
payment_type| tip        
------------| -----------
CASH        | 0.001723333
CREDIT      | 3.004373   
Dispute     | 0.02912136 
No Charge   | 0.0112184  
ooooooo
tip   
------
CREDIT
</code></pre>

</details>

⭐️
`Which vendor has the largest number of trips when only considering trips shorter than the average duration for each vendor?`
<details>
  <summary>Answer</summary>

<pre><code>/x
select sum trips by vendor where duration > (avg;duration) fby vendor from jan09 
select from jan09 where duration < (avg;duration) fby vendor 

show res8a:select count i from jan09 by vendor  where duration < (avg;duration) fby vendor 
/
error(if "from jan 09" and by vendor are opposite, it would work)
\

/▲
res8a:select from jan09 where duration < (avg;duration) fby vendor; 
res8b:select count i duration by vendor from res8a;
select vendor from res8b where duration = max duration;
/
vendor
------
VTS   
date       month   vendor pickup_time                   dropoff_time         ..
-----------------------------------------------------------------------------..
2009.01.10 2009.01 VTS    2009.01.10D00:00:00.000000000 2009.01.10D00:08:00.0..
2009.01.10 2009.01 VTS    2009.01.10D00:00:00.000000000 2009.01.10D00:04:00.0..
..
vendor| duration
------| --------
CMT   | 2883225 
DDS   | 402781  
VTS   | 3592460
\


/o
res7b:select count i by vendor from jan09 where duration < (avg;duration) fby vendor;
show res7b;
select vendor from res7b where x = max x
/
vendor| x      
------| -------
CMT   | 2883225
DDS   | 402781 
VTS   | 3592460
vendor
------
VTS
\

</code></pre>

</details>


## 2.5 Updating existing data

#### update
```q
// Find all records where the number of passengers is greater than 5
select passengers from jan09 where passengers > 5
/
passengers
----------
6         
6         
6         
6
..
\

jan09: update passengers: 5 from jan09 where passengers > 5
select passengers from jan09 where passengers > 5
/
passengers
----------
\

jan09:update wAvgfare:passengers wavg fare from jan09
meta jan09 //new column has been added to the end of the table
/
c           | t f a
------------| -----
date        | d    
month       | m    
..  
total       | f    
wAvgfare    | f
\
```

#### delete
```q
count jan09  //number of records before deleting rows
jan09:delete from jan09 where duration=00:00:00.000
count jan09  //number of records after deleteing rows 
/
10420159
10357181
\
```

## 2.6 Temporal Arithmetic(Math for time)

#### Casting
The `pickup_time` column in the data has a type of _timestamp_. As an example, we could convert the `pickup_time` values to their `minute` values (including hours and minutes), and group the data based on this time frame.
```q
/2way: a.b  or `b$a
select pickup_time, pickup_time.second, pickup_time.minute, `minute$pickup_time, pickup_time.hh from jan09
/
pickup_time                   second   minute pickup_time1 hh
-------------------------------------------------------------
2009.01.10D00:00:00.000000000 00:00:00 00:00  00:00        0 
2009.01.10D00:00:00.000000000 00:00:00 00:00  00:00        0
..
\
```

#### Order of Calculation
: sum fare + tip
This expression highlights an important feature of q syntax: **evaluation is from right-to-left**. The argument of `sum` is everything to its right, that is `fare` plus `tip. This simple rule holds everywhere; there are no priorities to remember.

```q
select total:sum fare + tip by pickup_time.minute from jan09
/
minute| total   
------| --------
00:00 | 81488.5 
00:01 | 81328.4
..
\

select total:(sum fare) + tip by pickup_time.minute from jan09 
/
minute| total                                                                ..
------| ---------------------------------------------------------------------..
00:00 | 77289.95 77289.95 77289.95 77289.95 77291.95 77289.95 77292.95 77293...
00:01 | 77106.05 77111.4 77106.05 77106.05 77106.05 77106.05 77108.05 77108.0..
..
\
```

#### [xbar](https://code.kx.com/q/ref/xbar/)
```q
/returns y rounded down to the nearest multiple of x. 
3 xbar 1 1 3 5 7 8 13
/
0 0 3 3 6 6 12
\

select count i by 60 xbar pickup_time.minute from jan09 where date = 2009.01.10
/
minute| x    
------| -----
00:00 | 28975
01:00 | 24007
02:00 | 20202
03:00 | 15472
04:00 | 9721
\
```
### Exercise 6
`Show the largest tip for each 15-minute timespan during the month of January.`
<details>
  <summary>Answer</summary>

<pre><code>/x
select max tip from jan09 15 xbar pickuptime.minute 
select max tip by 15 xbar pickup_time.minute from jan09 where date.month within 2009.01
/
minute| tip
------| ---
\

/o


select max tip by 15 xbar pickup_time.minute from jan09 where date within 2009.01.10 2009.01.31
/
minute| tip  
------| -----
00:00 | 90   
00:15 | 68   
00:30 | 62
\
</code></pre>

</details>

`Break this information down by vendor.`
<details>
  <summary>Answer</summary>

<pre><code>/x
select max tip by vendor, 15 xbar pickup_time.minute from jan09 
/
vendor minute| tip  
-------------| -----
CMT    00:00 | 35   
CMT    00:15 | 55   
CMT    00:30 | 39.98
CMT    00:45 | 55.33
\

/o
select max tip by 15 xbar pickup_time.minute, vendor from jan09 
/
minute vendor| tip  
-------------| -----
00:00  CMT   | 35   
00:00  DDS   | 23.2 
00:00  VTS   | 90   
00:15  CMT   | 55   
00:15  DDS   | 55   
00:15  VTS   | 68   
..
\
</code></pre>

</details>
