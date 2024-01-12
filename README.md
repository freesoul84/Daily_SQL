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
