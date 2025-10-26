# 3. Joins

#### Review
### exercise 7
`Display the max and min temperatures for NYC each week through January (For this query a week is just every 7 days)`

<details>
  <summary>Answer</summary>

<pre><code>/o
tables [ ]
cols weather
select max maxtemp, min mintemp by 7 xbar date from weather
//alternative way- select max maxtemp, min mintemp by date.week from weather
/
`smalltrips`trips`weather
`date`maxtemp`mintemp`avgtemp`departuretemp`hdd`cdd`precip`newsnow`snowdepth
date      | maxtemp mintemp
----------| ---------------
2008.12.27| 34      15     
2009.01.03| 43      25     
2009.01.10| 41      9      
2009.01.17| 47      6      
2009.01.24| 46      13     
2009.01.31| 27      20
\
</code></pre>

</details>

- [Left Join](https://code.kx.com/q/ref/lj/)
- [AJ (As-of) Join](https://code.kx.com/q/ref/aj/)

## 3.1 Left Join

`t1 lj t2`

```q
// Find the number of trips per day

jan09:select from trips where date within 2009.01.01 2009.01.31
jan09C:select trips: count i by date from jan09
jan09C
/
date      | trips 
----------| ------
2009.01.01| 327625
2009.01.02| 376708
2009.01.03| 432710
..
\
```

ooking more closely at `jan09C`, it doesn't look like a normal kdb+ table as there is a vertical line appearing between the columns `date` and `x`. This informs us that this table is actually a **keyed table** which we require to have in most kdb+ joins. But how do we create keyed tables explicitly ? Well, we have two choices:

1. Using the syntax above - we can use a by clause
2. Using [xkey](https://code.kx.com/q/ref/keys/#xkey) or [!(bang)](https://code.kx.com/q/ref/enkey/)

```q
`date xkey weather //we are keying on date 
1!weather          //we are keying on the first column 
3!weather          //we can key on N number of columns
/
date      | maxtemp mintemp avgtemp departuretemp hdd cdd precip newsnow snow..
----------| -----------------------------------------------------------------..
2009.01.01| 26      15      20.5    -12.9         44  0   0      0       0   ..
2009.01.02| 34      23      28.5    -4.8          36  0                  0   ..
..
date      | maxtemp mintemp avgtemp departuretemp hdd cdd precip newsnow snow..
----------| -----------------------------------------------------------------..
2009.01.01| 26      15      20.5    -12.9         44  0   0      0       0   ..
2009.01.02| 34      23      28.5    -4.8          36  0                  0   ..
..
date       maxtemp mintemp| avgtemp departuretemp hdd cdd precip newsnow snow..
--------------------------| -------------------------------------------------..
2009.01.01 26      15     | 20.5    -12.9         44  0   0      0       0   ..
2009.01.02 34      23     | 28.5    -4.8          36  0                  0   ..
..
\

/If we want to unkey a keyed table using the `!`:
0!jan09C
date       trips 
-----------------
2009.01.01 327625
2009.01.02 376708
..
```

⭐️
```q
// select date and precipitation from the weather table
// key the result on date
// join to the unkeyed table jan09C (0! unkeys the table)
jan09W:jan09C lj `date xkey select date, precip from weather 
jan09W
jan09W:jan09C lj select avg precip by date from weather //using the by clause to key
/
date      | trips  precip
----------| -------------
2009.01.01| 327625 0     
2009.01.02| 376708       
2009.01.03| 432710       
2009.01.04| 367525 0     
2009.01.05| 370901       
2009.01.06| 427394 0.08  
2009.01.07| 371043 1.19  
2009.01.08| 477502 0
..
\

/same result if the left hand table is unkeyed?
unkeyedJan09C:0!jan09C
unkeyedJan09C lj `date xkey select date, precip from weather
/
date       trips  precip
------------------------
2009.01.01 327625 0     
2009.01.02 376708       
2009.01.03 432710       
2009.01.04 367525 0
...
\
```

### Exercise 8
`Create a new join which joins the number of trips with the average temperature from the weather data, per day for the month of January`

<details>
  <summary>Answer</summary>

<pre><code>/o
tables []
cols weather
cols trips
jan09C
jan09C lj `date xkey select date, avgtemp from weather
/
`jan09`jan09C`jan09W`smalltrips`trips`unkeyedJan09C`weather
`date`maxtemp`mintemp`avgtemp`departuretemp`hdd`cdd`precip`newsnow`snowdepth
`date`date`month`vendor`pickup_time`dropoff_time`duration`passengers`distance..
date      | trips 
----------| ------
2009.01.01| 327625
2009.01.02| 376708
2009.01.03| 432710
..
date      | trips  avgtemp
----------| --------------
2009.01.01| 327625 20.5   
2009.01.02| 376708 28.5   
2009.01.03| 432710 33.5   
..
\
</code></pre>

</details>

