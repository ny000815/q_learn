## Data exploration

List existing tables:

```q
tables[]
```

Inspect:

```q
trips
```

Count rows:

```q
count trips
```

Schema:

```q
meta trips
cols trips
```

---

## qSQL Basics

Select all:

```q
select from smalltrips
```

Select columns:

```q
select vendor, pickup_time, fare from smalltrips
```

Filter with conditions:

```q
select date, month, vendor, passengers, fare, tip, fare+tip
from trips
where date = min date, tip > 20
```

Performance ordering matters (partit了解！
おそらく **上記の Jupyter Notebook チュートリアル全体を Markdown 形式に整形してほしい** という意味ですね。
（コードは `q ` や `python` ブロックに整えます）

以下、**内容を崩さず Markdown 化**しました。
必要であれば *短縮版 / 日本語版 / 英語版 / セクションごとの分割* も作成できます。

---

# kdb+/q Data Exploration — Markdown Notebook

## Setup

```python
#;.pykx.disableJupyter()

# https://code.kx.com/pykx/3.0/examples/jupyter-integration.html#q-first-mode
import pykx as kx
kx.util.jupyter_qfirst_enable()

# Jupyter notebook
# Shift-Enter to execute
```

```q
system"cd ",.trn.nbdir:$["/"=first v;"";getenv[`HOME],"/"],v:first system "dirname '",getenv[`JPY_SESSION_NAME],"'"
\l scripts/loaddata.q
```

---

## Learning objectives

To understand:

* qSQL query construction
* Variable assignment
* Built-in aggregations
* `by` grouping
* `fby` simplifieioned by `date`).

Measure timing:

```q
\ts select ... where tip > 20, date = min date
```

---

## Calculated columns

```q
select duration, total: fare + tip from trips
```

Count via virtual column `i`:

```q
select count i from trips where date = min date, passengers = 2
```

---

## Exercise 1

Trips with `<2 passengers` on earliest date:

```q
select count i from trips where date = min date, passengers < 2
```

On latest date:

```q
select count i from trips where date = max date, passengers < 2
```

---

## Exercise 2

```q
select payment_type, fare from trips where date = min date
```

---

## Assignment (`:`)

Store subset for January:

```q
jan09: select from trips where date within 2009.01.10 2009.01.31
count jan09
```

---

## Exercise 3

Store Exercise2 result:

```q
res2: select payment_type, fare from trips where date = min date
select from res2
```

---

## Aggregations

```q
select sum fare, sum tip from jan09
```

Other aggregations:

* `min`
* `max`
* `avg`
* `med`
* `count`

---

## Exercise 4

```q
select maxTip:max tip, minTip:min tip from jan09
```

---

## Grouping with `by`

Group fare by vendor:

```q
select fare by vendor from jan09
```

Aggregated:

```q
select sum fare, sum tip by vendor from jan09
```

Count per day:

```q
select count i by date from jan09
```

---

## Exercise 5 (largest tip per vendor)

```q
select max tip by vendor from jan09
```

---

## Exercise 6 (max & avg tip per payment_type)

```q
select maxTip:max tip, avgTip:avg tip by payment_type from jan09
```

---

## Using `fby`

Average duration comparison:

```q
select max fare from jan09 where duration < (avg;duration) fby vendor
```

---

## Exercise 7-a

Which payment type yields highest avg tip when fare > vendor avg fare?

```q
res7a: select avg tip by payment_type from jan09 where fare > (avg;fare) fby vendor
select payment_type from res7a where tip = max tip
```

## Exercise 7-b

Vendor with most short trips (less than avg vendor duration):

```q
res7b: select count i by vendor from jan09 where duration < (avg;duration) fby vendor
select vendor from res7b where x = max x
```

---

## Update existing data

Find suspect high passenger counts:

```q
select max passengers by vendor from jan09
```

Fix >5 passengers:

```q
jan09: update passengers:5 from jan09 where passengers > 5
```

Weighted average fare per passenger:

```q
jan09: update wAvgfare: passengers wavg fare from jan09
meta jan09
```

Delete zero-duration trips:

```q
jan09: delete from jan09 where duration=00:00:00.000
```

---

## Temporal arithmetic

Inspect timestamp fields:

```q
select pickup_time, pickup_time.second, pickup_time.minute, pickup_time.hh from jan09
```

Sum revenue per minute:

```q
select total: sum fare + tip by pickup_time.minute from jan09
```

Bucket minutes into hours:

```q
select count i by 60 xbar pickup_time.minute from jan09 where date = 2009.01.10
```

---

## Exercise 8-a

Largest tip per 15-minute window:

```q
select maxTip:max tip by 15 xbar pickup_time.minute from jan09
```

## Exercise 8-b

Breakdown by vendor:

```q
select maxTip:max tip by 15 xbar pickup_time.minute, vendor from jan09
```

---

