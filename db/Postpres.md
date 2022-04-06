# Postgres 问题

## 对于window function 提供和不提供 order by 的区别

```postgres
mydb=# select name,price,sum(price) over () from prices;
       name        |   price    |     sum
-------------------+------------+-------------
 GOLD.ROGER        | 5564800000 | 33495400000
 EDWARD.NEWGATE    | 5046000000 | 33495400000
 KAIDO             | 4611100000 | 33495400000
 CHARLOTTE.LINLIN  | 4388000000 | 33495400000
 SHANKS            | 4048900000 | 33495400000
 MARSHALL.D.TEACH  | 2247600000 | 33495400000
 MONKEY.D.LUFFY    | 1500000000 | 33495400000
 QUEEN             | 1320000000 | 33495400000
 KATAKURI          | 1057000000 | 33495400000
 JACK              | 1000000000 | 33495400000
 CHARLOTTE.CRACKER |  860000000 | 33495400000
 PEROSPERO         |  700000000 | 33495400000
 SABO              |  602000000 | 33495400000
 PORTGAS.D.ACE     |  550000000 | 33495400000
(14 rows)
```

不提供order by 则window frame 计算的值是整个分区的所有数据。

```postgres
mydb=# select name,price,sum(price) over (order by price) from prices;
       name        |   price    |     sum
-------------------+------------+-------------
 PORTGAS.D.ACE     |  550000000 |   550000000
 SABO              |  602000000 |  1152000000
 PEROSPERO         |  700000000 |  1852000000
 CHARLOTTE.CRACKER |  860000000 |  2712000000
 JACK              | 1000000000 |  3712000000
 KATAKURI          | 1057000000 |  4769000000
 QUEEN             | 1320000000 |  6089000000
 MONKEY.D.LUFFY    | 1500000000 |  7589000000
 MARSHALL.D.TEACH  | 2247600000 |  9836600000
 SHANKS            | 4048900000 | 13885500000
 CHARLOTTE.LINLIN  | 4388000000 | 18273500000
 KAIDO             | 4611100000 | 22884600000
 EDWARD.NEWGATE    | 5046000000 | 27930600000
 GOLD.ROGER        | 5564800000 | 33495400000
(14 rows)
```

计算的则是从分区的开始行到当前行的数据。
