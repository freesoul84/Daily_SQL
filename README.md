# Daily_SQL

## [User's Third Transaction (Uber SQL Interview Question)](https://datalemur.com/questions/sql-third-transaction) [Difficulty : Medium]

```
SELECT user_id, 
spend, 
transaction_date 
FROM (SELECT user_id, spend, transaction_date, ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY transaction_date) AS rn
FROM transactions) A 
WHERE rn = 3
```

--> Here I partitioned by user_id that means rank will be given to different transactions present for same user_id. and added transaction date order in ascending order so that transaction will be in a sequence.


## [Sending vs. Opening Snaps [Snapchat SQL Interview Question]](https://datalemur.com/questions/time-spent-snaps) [Difficulty : Medium]

```
SELECT B.age_bucket,
ROUND(SUM(CASE WHEN activity_type = 'send' THEN time_spent END)*100.0/SUM(time_spent),2) AS send_perc,
ROUND(SUM(CASE WHEN activity_type = 'open' THEN time_spent END)*100.0/SUM(time_spent),2) AS open_perc
FROM activities A
INNER JOIN age_breakdown B
ON A.user_id = B.user_id
WHERE A.activity_type IN ('open','send')
GROUP BY B.age_bucket
```

--> As we need to calculate sum only of 'send' and 'open' activity I considered only those activity_type rows.

--> Learning : use of CASE statement in AGGREGATE function in SELECT statement.


## [International Call Percentage [Verizon SQL Interview Question]](https://datalemur.com/questions/international-call-percentage) [Difficulty : Medium]

```
SELECT 
  ROUND(SUM(CASE
    WHEN (SELECT TRIM(country_id) 
          FROM phone_info 
          WHERE caller_id = A.caller_id) != 
          (SELECT TRIM(country_id) 
          FROM phone_info 
          WHERE caller_id = A.receiver_id) THEN 1 ELSE  0
          END)*100.0/COUNT(A.caller_id),1) AS C
FROM phone_calls A
```

--> Learning : Used comparison of values in a select statement using sub queries where the value we are matching from the main query.
