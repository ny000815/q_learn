# join
review: how to key the table with the first line
1!trade

lj. Be careful not to overwrite when you have same column.
ij, with sublist(#), you can narrow down

```q
/cross
show rack:([]time:09:00 09:30 10:00) cross ([]sym:`JPM`AAPL)
/
time  sym 
----------
09:00 JPM 
09:00 AAPL
09:30 JPM 
09:30 AAPL
10:00 JPM 
10:00 AAPL
\

/aj
/💡No need to be keyed for aj
quote
/
time  sym  ask   bid  
----------------------
09:29 JPM  30.23 30.2 
09:29 AAPL 40.2  40.19
09:32 JPM  30.35 30.33
09:33 AAPL 40.35 40.32
\
aj[`sym`time;rack;quote]
/
time  sym  ask   bid  
----------------------
09:00 JPM             
09:00 AAPL            
09:30 JPM  30.23 30.2 
09:30 AAPL 40.2  40.19
10:00 JPM  30.35 30.33
10:00 AAPL 40.35 40.32
\
```
Combination with update table
```q
quote
/

\
update time -5 from quote
/

\
```

```q
/違い
show b:(sym:`e`f!price:5 6f)
/
e| 5
f| 6
\
show b:(`sym`price!(`e`f;5 6f))
/
sym  | e f
price| 5 6
\
```
xcol
```q
table:([]a:1 2 3;z:`a`b`c)
`b xcol table
/
b z
---
1 a
2 b
3 c
\
```
\# for repeating & using ; and () to use namespace
```q
show p:([]Name:4#`Bob;Date:(2025.01.01;2025.01.02;2025.01.03;.z.d)
/
Name Date
-------------
Bob  2025.01.01
Bob  2025.01.02
Bob  2025.01.03
Bob  202X.01.01
\
```

confusion of inner join with union join

⭐️get, update, \`g (grouped attribute), from get
- Returns a join of the trade and quote table, which shows each trade record with the prevailing quote (on any exchange) at the time of each trade
- Apply grouped attribute for performance
- Quotes should be specific to the sym
- Assumes global table trade and quote
```q
getPrevailingQuote:{
    aj[`sym`time;update `g#sym from get `trade; get `quote]
    }
```
