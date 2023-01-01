---
title: MySQL Lock Mode
description: 簡單介紹及翻譯 MySQL Lock Mode
tags:
  - MySQL
  - DB
categories:
  - MySQL
created: 2023-01-01T09:25:19.191Z
modified: 2023-01-01T09:25:20.617Z
---

# mysql lock mode

Status: Not started
Tags: MySQL

[MySQL :: MySQL 8.0 Reference Manual :: 15.7.1 InnoDB Locking](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html)

InnoDB 的 lock

主要分成這幾種

# • [Shared and Exclusive Locks](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html#innodb-shared-exclusive-locks)

InnoDB 在 row-level 的 lock 有兩種不同的類型

- Shared(S) locks - 讓持有該 lock 的 transaction 能讀取這行 row 的資料
- Exclusive(X) locks - 讓持有該 lock 的 transaction 能修改 刪除這行 row 的資料

其中 S  lock 是可以共享的， X lock 則不能

舉些例子

1. 如果 transaction t1 已經有 row r 的 S lock，那這時 transaction t2 要求 row r 的 lock 時
    - 如果 t2 要求 row r 的 S lock，則兩個. transaction 都能拿到 S lock， S lock 互相不排斥
    - 如果 t2 要求 row r 的 X lock，則 t2 需要等到 t1 做完後才能拿到 row r 的 X lock
2. 如果 transaction t1 已經有 row r 的 X lock，那這時無論 transaction t2 要求 X lock 或是 S lock 都需要等 t1 做完後才能拿到

# • [Intention Locks](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html#innodb-intention-locks)

InnoDB 支援不同顆粒度的鎖，像是 row lock，或者是 table lock

那假設現在 transaction t1 想要對整張表 table1 加上 X lock，此時 InnoDB 會需要先檢查 table1 這張表的每個 record 是否已經有被其他 transaction lock 了，才能決定是否要給 transaction t1 表級別的  lock。

但是，要掃描整張表的 record lock 效率太差了，因此才誕生出 intention lock，讓 InnoDB 能更好的判斷是否能夠給 transaction table 等級的 lock

Intention lock 表示某個 transaction 想要再這個 table 的某個 record 上加上鎖

因此

- [intention shared lock](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_intention_shared_lock) (IS) 表示這張表上，有個 transaction 想要拿到某個 row 的 S lock
- [intention exclusive lock](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_intention_exclusive_lock) (IX) 表示這張表上，有個 transaction 想要拿到某個 row 的 X lock

首先，先確立兩點規則

1. 任何 transaction 要拿 row S lock 時，必須先拿到該 table 的 intention share(IS) lock 或是更強的 lock
2. 任何 transaction 要拿 row X lock 時，必須先拿到該 table 的 intention exclusive(IX) lock

因此，如果一個 transaction 想要拿到 table 等級的 X 或 S lock，則我們只需要檢查該 table 有沒有被其他 transaction 加上 IX 或 IS lock，就能知道該不該給 table 等級的 X 或 S lock

也因此，IX 跟 IS 相互之間是不衝突的，只有當 table 等級的 S 或 X lock 來時，才要判斷是否能給 lock，下面是 table 級別 lock 的兼容性

|     | X        | IX         | S          | IS         |
| --- | -------- | ---------- | ---------- | ---------- |
| X   | Conflict | Conflict   | Conflict   | Conflict   |
| IX  | Conflict | Compatible | Conflict   | Compatible |
| S   | Conflict | Conflict   | Compatible | Compatible |
| IS  | Conflict | Compatible | Compatible | Compatible |

# • [Record Locks](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html#innodb-record-locks)

在 InnoDB 中 row lock 其實就是把該 row 的 [cluster index](https://dev.mysql.com/doc/refman/8.0/en/innodb-index-types.html) 給 lock 住，因此 InnoDB 的 row lock 其實就是 record lock.

如果說該 table 沒有 cluster index，那 InnoDB 會自己建一個隱藏的 [cluster index](https://dev.mysql.com/doc/refman/8.0/en/innodb-index-types.html)，並 lock 對應 row 的 index

# • [Gap Locks](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html#innodb-gap-locks)

用來鎖 兩個 index 中間範圍的鎖

例如下面這句執行時，會把 10 ~ 20 範圍內的鎖全部鎖起來，防止有其他 transaction 在中間插入新的 row

`SELECT c1 FROM t WHERE c1 BETWEEN 10 and 20 FOR UPDATE;`

這樣做是為了防止 `幻讀 (Phantom Reads)` 發生，亦即 t1 讀取同樣範圍兩次，但 t2 在這之間插入了一筆資料，這時候 t1 兩次讀出來的行數會不一樣，這就是 `幻讀 (Phantom Reads)`

也因為這是為了防止 幻讀 (Phantom Reads) 的發生，因此如果把 transaction isolation level 切成 `[READ COMMITTED](https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-isolation-levels.html#isolevel_read-committed)` 的話，就不會產生 gap lock

Gap lock 對於 unique index 無效，但對其他類型的 index 有效

例如下面這句

`SELECT * FROM child WHERE id = 100;`

如果 id 不是 `unique index` 的話，那 100 之前到下一個有效 index (例如 90) 之間的範圍 (91 ~ 100)會被鎖起來，然而，如果是 `unique index` ，則只會鎖上這行而已

此外，比較有趣的一點是  gap lock 是可以被多個 transaction 同時持有的，因為 gap lock 之間不衝突，都是執行同樣的功能 (防止 record 遭到變化)，也因此， S gap lock  跟 X gap lock 沒有區別

更多例子可以參考

[带你了解record lock、gap lock、next-key lock - 掘金 (juejin.cn)](https://juejin.cn/post/7018137095315128328)

[鎖( Lock )的介紹與死鎖分析](https://blog.twjoin.com/%E9%8E%96-lock-%E7%9A%84%E4%BB%8B%E7%B4%B9%E8%88%87%E6%AD%BB%E9%8E%96%E5%88%86%E6%9E%90-19833c18baab)

# • [Next-Key Locks](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html#innodb-next-key-locks)

就是 record lock + gap lock，例如

`SELECT * FROM child WHERE id = 100;`

如果 id 不是 `unique index` 的話，那 100 之前到下一個有效 index (例如 90) 之間的範圍 (91 ~ 100)會被鎖起來，然而，如果是 `unique index` ，則只會鎖上這行而已

# • [Insert Intention Locks](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html#innodb-insert-intention-locks)

是 gap lock 的一種，在插入前會要先取得，主要是讓兩個插入可以不用等待

例如 gap 是 10 ~ 20，這時候如果一個要插入 13 另一個插入 17 的話，這兩個插入就不會衝突，因此都可以拿到 Insert Intention lock

# • [AUTO-INC Locks](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html#innodb-auto-inc-locks)

當插入有 AUTO_INCREMENT 的 table 時，則會把整張表 lock 住，其他想插入的 transaction 必須等到做完後才能繼續做，這樣做是為了取得連續的 index

我們可以透過 變數 `[innodb_autoinc_lock_mode](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_autoinc_lock_mode)` 來控制 lock 的行為，詳細可以看 [Section 15.6.1.6, “AUTO_INCREMENT Handling in InnoDB”](https://dev.mysql.com/doc/refman/8.0/en/innodb-auto-increment-handling.html)

# • [Predicate Locks for Spatial Indexes](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html#innodb-predicate-locks)

# 延伸閱讀

- [你的insert死锁了看这里 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/222111898)
- https://github.com/octachrome/innodb-locks
-