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


## [Card Launch Success [JPMorgan Chase SQL Interview Question]](https://datalemur.com/questions/card-launch-success) [Difficulty : Medium]

```
-- find the launch month of the credit card : filtering
-- count total card issued in that month : counting and summing
-- order the result by count obtained in second line

;WITH CTE1 AS(SELECT distinct card_name,
MIN(issue_year) OVER(PARTITION BY card_name ORDER BY issue_year, issue_month) AS earliar_year,
MIN(issue_month) OVER(PARTITION BY card_name ORDER BY issue_year, issue_month) AS earliar_month
FROM monthly_cards_issued)

SELECT A.card_name, B.issued_amount FROM
(SELECT card_name, earliar_year, earliar_month FROM CTE1 ) A
INNER JOIN 
monthly_cards_issued B 
ON A.card_name = B.card_name 
WHERE A.earliar_month = B.issue_month AND A.earliar_year = B.issue_year
ORDER BY B.issued_amount DESC
```

--> Learning : Use of Window function and CTE


## [Compressed Mode [Alibaba SQL Interview Question]](https://datalemur.com/questions/alibaba-compressed-mode) [Difficulty : Medium]

```
SELECT A.item_count FROM 
items_per_order A
WHERE A.order_occurrences = (SELECT MAX(order_occurrences) 
FROM items_per_order) 
ORDER BY A.item_count ASC
```

--> Learning : Used subquery in where clause