# 11.Queries
- How to construct a qSQL query
- The four different qSQL queries - `select`,`exec`,`update` and `delete`
- Building queries with constraints
- Building queries with aggregations
- Building queries with grouping
- Updating existing data
- Deleting existing data
- Using `fby`

```q
tables[]! count each value each tables[]          //A quick shortcut to see each table and the associated table counts 

```

where clause
- If a date filter exists, place it first
- `,` performs faster than `and`

[wavg](https://code.kx.com/q/ref/avg/#wavg)
size
```q
select vwap: size wavg price from trade where sym = `GOOG
```

selecting by giving arg
```q
(select max size by sym from daily)`IBM    //getting the max size for IBM
```
⭐️?
```q
last5:{-5 sublist raze x}
select last5DaysClose:last5 close by sym from daily
```
⭐️xbar
```q
select trds:count i, vwap:size wavg price by sym, 15 xbar time.minute from trade where date = last date 
```

by 00:01:30
```q
select by `time$0D00:01:30.000 xbar `timespan$time from trade
```

Use `xbar` to generate a count of the number of trades (`trade where date = last date`) in intervals of trade size (interval size 10).(_This is commonly used to generate a histogram of trade size distribution_)
```q
select count i by 10 xbar size from trade where date = last date
```

### exec
useful to get whole column
```q
exec size from daily 
/
536408 532160 530579 531534 539534 535376 524307 531573 538767 545950 533825 ..
\
exec 3 sublist price, 3 sublist size by sym from daily //sublisting for visibility
/
    | price                                  size                
----| -----------------------------------------------------------
AAPL| 4.452568e+07 4.93008e+07  4.673882e+07 536408 554761 535121
AIG | 1.515896e+07 1.664099e+07 1.640576e+07 532160 559732 544834
AMD | 1.744796e+07 2.020541e+07 1.776361e+07 530579 552365 542053
DELL| 6657170      6673491      6426205      531534 543730 546929
DOW | 1.101615e+07 1.146551e+07 1.138643e+07 539534 552440 537432
GOOG| 3.800257e+07 3.944743e+07 3.765593e+07 535376 551302 526374
HPQ | 1.804498e+07 1.957508e+07 1.839489e+07 524307 563705 528128
IBM | 2.267041e+07 2.358848e+07 2.339335e+07 531573 550613 528404
INTC| 2.638187e+07 2.758548e+07 2.748836e+07 538767 553318 542887
MSFT| 1.650542e+07 1.519559e+07 1.538936e+07 545950 544680 525033
ORCL| 1.823452e+07 1.909067e+07 2.007516e+07 533825 542955 536128
PEP | 1.176689e+07 1.236042e+07 1.181257e+07 525361 555637 536426
PRU | 3.170256e+07 3.393572e+07 2.984919e+07 543107 561256 527113
SBUX| 3.407796e+07 3.486188e+07 3.377464e+07 530383 563825 531627
TXN | 9647702      9965824      9498755      529999 553721 535724
\
```
pq
```q
0!exec first open, last close by sym:sym from daily where sym like "*L"
/sym:sym is the point. without it, it would be a dict. not a keyed table


/
sym  open  close
----------------
AAPL 83.88 90.95
DELL 12    13.29
ORCL 35    42.82
\
/this works too but this time, but this is question for exec
0!select first open, last close by sym from daily where sym like "*L"
```

passing reference vs passing value
```q
delte from daily where sym = `AAPL
/OG one is not delete
delte from `daily where sym = `AAPL
/OG one is delted
```

fby
```q
select from trade where size > (avg;size) fby sym 
```

fby without where pattern
```q
select sym, size, ex, lessThanEx: size < (avg;size) fby ex from trade
```

# Ex
- xbar is after by
```q
select by 10 xbar time.minute, sym from trade
```
- using <> instead of not a=b.
- using in for where clause
```q
select from trade where sym in `JPM`AAPL
```
- how to create table
- wavg
```q
size wavg price
```

- by 1 minute with time type
```q
/possible to cast after by
`time$time.minute


/if it is 1, no need to use xbar
by 1 xbar time.minute
/equals
by time,minute
```

- Remember to cast when you manipulate int with decimals
```q
update `int$price % 0.78
```
