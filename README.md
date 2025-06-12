# Unique_project

## 📁 Dataset 1: *E-commerce Orders*

| order\_id | customer\_id | order\_date | product\_id | amount | status    |
| --------- | ------------ | ----------- | ----------- | ------ | --------- |
| 1001      | C101         | 2024-01-05  | P01         | 250.00 | Delivered |
| 1002      | C102         | 2024-01-06  | P02         | 450.00 | Cancelled |
| 1003      | C103         | 2024-01-07  | P03         | 150.00 | Returned  |
| ...       | ...          | ...         | ...         | ...    | ...       |

1. Find the total number of orders placed in January 2024.
query:-
select count(*) from `example1.demo` where order_date >= '2024-01-01' and order_date<'2024-02-01'and 
status = "Delivered";

2. List the top 3 products by total revenue.
query:-
select product_id,amount from `unique1-462503.example1.demo` order by amount desc limit 3;

3. Calculate the average order value for each customer.
query:-
select customer_id,avg(amount) from `unique1-462503.example1.demo` group by customer_id;

4. Get daily order counts and plot trend (optional visualization).
query:-
select order_date,count(*) as total_order 
from `unique1-462503.example1.demo`
group by order_date;

5. Show the % of delivered, cancelled, and returned orders.
query:-
select status,COUNT(*) * 100.0 / 
(SELECT COUNT(*) FROM `unique1-462503.example1.demo` ) AS percentage 
from `unique1-462503.example1.demo` group by status;



## 📁 Dataset 2: *Employee Attendance*

| emp\_id | name  | department | date       | status  |
| ------- | ----- | ---------- | ---------- | ------- |
| E101    | Ravi  | IT         | 2024-06-01 | Present |
| E102    | Seema | HR         | 2024-06-01 | Absent  |
| E103    | Sunil | IT         | 2024-06-01 | Present |
| ...     | ...   | ...        | ...        | ...     |


1. Count total working days for each employee in June 2024.
query:-
select emp_id,count(*) as working_day from `unique1-462503.example1.EmployeeAttendance` 
where status = "Present" and  date>= '2024-06-01'
and date<'2024-06-30' group by emp_id order by emp_id;

2. List employees with highest absenteeism.
query:-
select emp_id,count(*) as working_day from `unique1-462503.example1.EmployeeAttendance` where status = "Absent"
group by emp_id order by emp_id desc;

3. Calculate attendance % per department.
query:-
 select department,count(*) * 100.0 / (select count(*) from `unique1-462503.example1.EmployeeAttendance`)
as working_day from `unique1-462503.example1.EmployeeAttendance` where status = "Present" group by
department;

4. Find employees who were never absent.
query:-
select emp_id,count(*) as Absent from `unique1-462503.example1.EmployeeAttendance`
where status = "Absent" group by emp_id;

5. Get day-wise attendance summary.
query:-
select date,count(distinct emp_id) as total_present from `unique1-462503.example1.EmployeeAttendance`
where status = "Present" and date between '2024-06-01' and '2024-06-30' group by date order by date; 




## 📁 Dataset 3: *YouTube Video Analytics*

| video\_id | channel\_name | views  | likes | comments | upload\_date |
| --------- | ------------- | ------ | ----- | -------- | ------------ |
| V101      | TechWorld     | 100000 | 5000  | 800      | 2024-04-01   |
| V102      | FitLife       | 200000 | 15000 | 2500     | 2024-04-05   |
| ...       | ...           | ...    | ...   | ...      | ...          |


1. Get top 5 videos by engagement (likes + comments).
query:- 
select video_id,likes,comments from
 `unique1-462503.example1.YouTubeAnalytics`
order by likes desc;

2. Calculate average views per channel.
query:-
select channel_name,avg(views) as viewss 
from `unique1-462503.example1.YouTubeAnalytics`
group by channel_name;

3. Identify videos with a like-to-view ratio < 5%.
query:-
select video_id from `unique1-462503.example1.YouTubeAnalytics`
where (likes*100) / nullif(views,0) <= 5;

4. Find channel with the most uploaded videos in April.
query:-
select channel_name,count(*) as most_uploaded_video
 from `unique1-462503.example1.YouTubeAnalytics`
where upload_date between '2024-04-01' and '2024-04-30'
 group by channel_name order by channel_name desc;

5. Rank videos based on comments.
query:-
 select video_id,comments
  from `unique1-462503.example1.YouTubeAnalytics`
order by comments desc;



## 📁 Dataset 4: *Bank Transactions*

| txn\_id | account\_id | txn\_date  | txn\_type | amount |
| ------- | ----------- | ---------- | --------- | ------ |
| T1001   | A101        | 2024-05-01 | Credit    | 5000   |
| T1002   | A101        | 2024-05-02 | Debit     | 2000   |
| T1003   | A102        | 2024-05-03 | Debit     | 1500   |
| ...     | ...         | ...        | ...       | ...    |


1. Calculate monthly credit and debit totals per account.
query:-
select account_id,extract(month from txn_date) as month, 
sum(case when txn_type = "Credit" then amount else 0 end) as total_credit,
sum(case when txn_type = "Debit" then amount else 0 end) as total_debit 
from `unique1-462503.example1.BankTransactions`
group by account_id,month;

2. List accounts with more debits than credits.
query:-
SELECT account_id,
SUM(CASE WHEN txn_type = 'debit' THEN amount ELSE 0 END) AS total_debits,
SUM(CASE WHEN txn_type = 'credit' THEN amount ELSE 0 END) AS total_credits
FROM `unique1-462503.example1.BankTransactions`
GROUP BY  account_id
HAVING total_debits>total_credits;

3. Get top 5 highest single transaction amounts.
query:-
select *
 from `unique1-462503.example1.BankTransactions`  order by amount desc;

4. Find the balance for each account (credit - debit).
query:-
select account_id,
SUM(CASE WHEN txn_type = 'credit' THEN amount ELSE 0 END) AS total_credits,
SUM(CASE WHEN txn_type = 'debit' THEN amount ELSE 0 END) AS total_debits,
SUM(CASE WHEN txn_type = 'credit' THEN amount ELSE 0 END) -
SUM(CASE WHEN txn_type = 'debit' THEN amount ELSE 0 END) AS balance
from `unique1-462503.example1.BankTransactions`
group by account_id;

5. Detect transactions over ₹10,000 for fraud check.
query:-
select * from `unique1-462503.example1.BankTransactions`
where amount>10000 order by amount desc;




## 📁 Dataset 5: *StackOverflow Developer Survey*

| respondent\_id | language   | country | experience\_level | salary\_usd |
| -------------- | ---------- | ------- | ----------------- | ----------- |
| 1              | Python     | India   | Mid               | 15000       |
| 2              | Java       | USA     | Senior            | 120000      |
| 3              | JavaScript | UK      | Junior            | 40000       |
| ...            | ...        | ...     | ...               | ...         |


1. Count how many developers use each language.
query:-
select language,count(*) as developers from `unique1-462503.example1.StackOverflow`
group  by language;

2. Average salary per experience level.
query:-
select experience_level,avg(salary_usd) as salary from `unique1-462503.example1.StackOverflow`
group by experience_level;

3. Top 5 countries by average salary.
query:-
select country,avg(salary_usd) as salary from `unique1-462503.example1.StackOverflow`
group by country order by salary desc;


4. Most popular language among juniors.
query:-
select distinct language from `unique1-462503.example1.StackOverflow`
where experience_level>"Junior";

5. Salary comparison: Python vs Java developers.
query:-
select language,sum(salary_usd) as salary from `unique1-462503.example1.StackOverflow`
where language in("Python","Java") group by language;




