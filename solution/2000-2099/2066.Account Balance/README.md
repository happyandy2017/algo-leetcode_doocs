# [2066. 账户余额](https://leetcode.cn/problems/account-balance)

[English Version](/solution/2000-2099/2066.Account%20Balance/README_EN.md)

## 题目描述

<!-- 这里写题目描述 -->

<p>表名: <code>Transactions</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| account_id  | int  |
| day         | date |
| type        | ENUM |
| amount      | int  |
+-------------+------+
(account_id, day) 是该Transactions表的主键.
表中的每行数据表示一次交易的信息, 包括此次交易的账号(account_id), 交易类型(type), 交易发生时间(day), 交易发生金额(amount).
其中交易类型(type)字段包括了两种行为：存入 ('Deposit'), 取出('Withdraw').
</pre>

<p>&nbsp;</p>

<p>请写出能够返回用户每次交易完成后的账户余额. 我们约定所有用户在进行交易前的账户余额都为0，&nbsp;并且保证所有交易行为后的余额不为负数。</p>

<p>返回的结果请依次按照 账户（<code>account_id</code>),&nbsp;日期(&nbsp;<code>day</code>&nbsp;) 进行<strong>升序排序</strong>&nbsp;.</p>

<p>查询结果的格式请参照以下测试样例.</p>

<p>&nbsp;</p>

<p><strong>测试样例1:</strong></p>

<pre>
<strong>输入:</strong> 
Transactions 表:
+------------+------------+----------+--------+
| account_id | day        | type     | amount |
+------------+------------+----------+--------+
| 1          | 2021-11-07 | Deposit  | 2000   |
| 1          | 2021-11-09 | Withdraw | 1000   |
| 1          | 2021-11-11 | Deposit  | 3000   |
| 2          | 2021-12-07 | Deposit  | 7000   |
| 2          | 2021-12-12 | Withdraw | 7000   |
+------------+------------+----------+--------+
<strong>输出:</strong> 
+------------+------------+---------+
| account_id | day        | balance |
+------------+------------+---------+
| 1          | 2021-11-07 | 2000    |
| 1          | 2021-11-09 | 1000    |
| 1          | 2021-11-11 | 4000    |
| 2          | 2021-12-07 | 7000    |
| 2          | 2021-12-12 | 0       |
+------------+------------+---------+

<strong>解释:</strong> 
账户1:
- 初始金额为 0.
- 2021-11-07 --&gt; 存入2000. 余额变为 0 + 2000 = 2000.
- 2021-11-09 --&gt; 取出1000. 余额变为 2000 - 1000 = 1000.
- 2021-11-11 --&gt; 存入3000. 余额变为 1000 + 3000 = 4000.
账户2:
- 初始金额为 0.
- 2021-12-07 --&gt; 存入7000. 余额变为 0 + 7000 = 7000.
- 2021-12-12 --&gt; 取出 7000. 余额变为 7000 - 7000 = 0.
</pre>

## 解法

### 方法一

<!-- tabs:start -->

```sql
# Write your MySQL query statement below
SELECT
    account_id,
    day,
    SUM(IF(type = 'Deposit', amount, -amount)) OVER (
        PARTITION BY account_id
        ORDER BY day
    ) AS balance
FROM Transactions
ORDER BY 1, 2;
```

<!-- tabs:end -->

<!-- end -->
