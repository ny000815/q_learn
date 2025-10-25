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
On the earliest date, how many trips had fewer than two passengers?
<details>
  <summary>Answer</summary>

<pre><code>/x
select count i from trips where date = min date, passengers < 2
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

