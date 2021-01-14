1a.
```sql
SELECT
company_id,
login_7_day login_cnt,
'7_day' login_flag
FROM login_table
UNION
SELECT
company_id
, login_30_day login_cnt
, '30_day' login_flag
FROM login_table
ORDER BY login_cnt;
```
1b.
```sql
SELECT a.company_id
, a.login_cnt AS login_7_day
, b.login_cnt AS login_30_day
FROM login_table_transpose AS a
JOIN login_table_transpose AS b
ON a.company_id = b.company_id
AND a.login_flag != b.login_flag
AND a.login_cnt = (
SELECT login_cnt
FROM login_table_transpose
where login_flag = '7_day')
AND b.login_cnt = (
select login_cnt
FROM login_table_transpose
WHERE login_flag = '30_day'
)
```
2.
```sql
select date_part('month', billing_failure_date) mos
, company_id
, count(*) failure_num
from (
select company_id, billing_failure_date
from billing_table
where billing_failure_date + 3 not in
(select billing_failure_date from billing_table)
and billing_failure_date - 3 not in
(select billing_failure_date from billing_table)
) as legit_cancels
group by date_part('month', billing_failure_date), company_id
order by date_part('month', billing_failure_date)```
