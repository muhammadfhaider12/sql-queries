# sql-queries  
This repository contains my solutions to the SQL challenges from [LeetCode SQL 50](https://leetcode.com/studyplan/top-sql-50/) 

### Q1:  
_SELECT product_id
FROM Products
WHERE low_fats = 'Y' AND recyclable = 'Y';_

### Q2:  
_SELECT name
FROM Customer
WHERE referee_id <> 2 OR referee_id IS NULL;_

### Q3:  
_SELECT name, population, area
FROM World
WHERE area >= 3000000 OR population >= 25000000;_  

### Q4:  
_SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id
ORDER BY id ASC;_  

### Q5:  
_SELECT tweet_id
FROM Tweets
WHERE LENGTH(content) > 15;_  

### Q6:
_SELECT eu.unique_id, e.name
FROM Employees e
LEFT JOIN EmployeeUNI eu ON e.id = eu.id;_  


### Q7:  
_SELECT p.product_name, s.year, s.price
FROM Sales s
JOIN Product p ON s.product_id = p.product_id;_    

### Q8:
_SELECT v.customer_id, COUNT(v.visit_id) AS count_no_trans
FROM Visits v
LEFT JOIN (
    SELECT visit_id
    FROM Transactions
) t ON v.visit_id = t.visit_id
WHERE t.visit_id IS NULL
GROUP BY v.customer_id;_    

### Q9:
_SELECT w2.id
FROM Weather w1
JOIN Weather w2 ON w1.recordDate = DATE_SUB(w2.recordDate, INTERVAL 1 DAY)
WHERE w2.temperature > w1.temperature;_  


### Q10:  
_SELECT machine_id, ROUND(AVG(timestamp_diff), 3) AS processing_time
FROM (
    SELECT machine_id, process_id,
           AVG(CASE WHEN activity_type = 'end' THEN timestamp END) -
           AVG(CASE WHEN activity_type = 'start' THEN timestamp END) AS timestamp_diff
    FROM Activity
    GROUP BY machine_id, process_id
) AS subquery
GROUP BY machine_id;_  
  
### Q11:
_SELECT e.name, b.bonus
FROM Employee e
LEFT JOIN Bonus b ON e.empId = b.empId
WHERE b.bonus < 1000 OR b.bonus IS NULL;_  

### Q12:  
_SELECT s.student_id, s.student_name, su.subject_name,
       COUNT(e.student_id) AS attended_exams
FROM Students s
CROSS JOIN Subjects su
LEFT JOIN Examinations e ON s.student_id = e.student_id AND su.subject_name = e.subject_name
GROUP BY s.student_id, s.student_name, su.subject_name
ORDER BY s.student_id, su.subject_name;_  
  
### Q13:
_SELECT e1.name
FROM Employee e1
JOIN (
    SELECT managerId, COUNT(*) AS direct_reports
    FROM Employee
    WHERE managerId IS NOT NULL
    GROUP BY managerId
) e2 ON e1.id = e2.managerId
WHERE e2.direct_reports >= 5;_  
  
### Q14:
_SELECT s.user_id,
       ROUND(COALESCE(SUM(CASE WHEN c.action = 'confirmed' THEN 1 ELSE 0 END) / NULLIF(COUNT(c.user_id), 0), 0), 2) AS confirmation_rate
FROM Signups s
LEFT JOIN Confirmations c ON s.user_id = c.user_id
GROUP BY s.user_id;_
  
### Q15:  
_SELECT id, movie, description, rating
FROM Cinema
WHERE id % 2 = 1 AND description != 'boring'
ORDER BY rating DESC;_  
  
### Q16:  
_SELECT
U.product_id,
ROUND(SUM(U.units * P.price) / SUM(U.units),2) AS average_price
FROM UnitsSold U
INNER JOIN Prices P
ON U.product_id = P.product_Id
WHERE U.purchase_date BETWEEN P.start_date AND P.end_date
GROUP BY 1;_  
  
### Q17:  
_SELECT project_id,
       ROUND(AVG(e.experience_years), 2) AS average_years
FROM Project p
JOIN Employee e ON p.employee_id = e.employee_id
GROUP BY project_id;_  
  
### Q18:  
_SELECT 
    contest_id,
    ROUND((COUNT(user_id) / (SELECT COUNT(DISTINCT user_id) FROM Users)) * 100, 2) AS percentage
FROM Register
GROUP BY contest_id
ORDER BY percentage DESC, contest_id ASC;_  
  
### Q19:  
_SELECT 
    query_name,
    ROUND(AVG(CAST(rating AS DECIMAL) / position), 2) AS quality,
    ROUND((COUNT(CASE WHEN rating < 3 THEN 1 END) / COUNT(*)) * 100, 2) AS poor_query_percentage
FROM Queries
WHERE query_name IS NOT NULL
GROUP BY query_name
ORDER BY quality DESC, query_name ASC;_  

### Q20:  
_SELECT
    DATE_FORMAT(trans_date, '%Y-%m') AS month,
    country,
    COUNT(id) AS trans_count,
    SUM(CASE WHEN state = 'approved' THEN 1 ELSE 0 END) AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM
    Transactions
GROUP BY
    DATE_FORMAT(trans_date, '%Y-%m'),
    country
ORDER BY
    month,
    country;_  
      
### Q21:        
_SELECT
    ROUND((SUM(CASE WHEN d.order_date = d.customer_pref_delivery_date THEN 1 ELSE 0 END) / COUNT(d.customer_id)) * 100, 2) AS immediate_percentage
FROM
    (SELECT
        customer_id,
        MIN(order_date) AS first_order_date
    FROM
        Delivery
    GROUP BY
        customer_id) AS first_orders
JOIN
    Delivery AS d ON first_orders.customer_id = d.customer_id AND first_orders.first_order_date = d.order_date;_  

### Q22:  
_SELECT ROUND(
    COUNT(DISTINCT first_logins.player_id) / (SELECT COUNT(DISTINCT player_id) FROM Activity),
    2
) AS fraction
FROM (
    SELECT player_id, MIN(event_date) AS first_login_date
    FROM Activity
    GROUP BY player_id
) AS first_logins
JOIN Activity AS next_day_logins
ON first_logins.player_id = next_day_logins.player_id
AND DATE_ADD(first_logins.first_login_date, INTERVAL 1 DAY) = next_day_logins.event_date;_  

### Q23:
_SELECT teacher_id, COUNT(DISTINCT subject_id) AS cnt
FROM Teacher
GROUP BY teacher_id;_  

### Q24:  
_SELECT 
    activity_date AS day, 
    COUNT(DISTINCT user_id) AS active_users
FROM 
    Activity
WHERE 
    activity_date BETWEEN DATE_SUB('2019-07-27', INTERVAL 29 DAY) AND '2019-07-27'
GROUP BY 
    activity_date;_  
  
### Q25:  
_SELECT 
    s.product_id,
    s.year AS first_year,
    s.quantity,
    s.price
FROM 
    Sales s
JOIN 
    Product p ON s.product_id = p.product_id
WHERE
    (s.product_id, s.year) IN (
        SELECT 
            product_id,
            MIN(year) AS first_year
        FROM 
            Sales
        GROUP BY 
            product_id
    );_  

### Q26:  
_SELECT class
FROM Courses
GROUP BY class
HAVING COUNT(student) >= 5;_  

### Q27:  
_SELECT user_id, COUNT(follower_id) AS followers_count
FROM Followers
GROUP BY user_id
ORDER BY user_id ASC;_  

### Q28:  
_SELECT MAX(num) AS num
FROM (
    SELECT num
    FROM MyNumbers
    GROUP BY num
    HAVING COUNT(num) = 1
) AS single_numbers;_  

### Q29:  
_SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(*) FROM Product);_  

### Q30:  
_SELECT e.employee_id, e.name, COUNT(r.employee_id) AS reports_count, ROUND(AVG(r.age)) AS average_age
FROM Employees e
LEFT JOIN Employees r ON e.employee_id = r.reports_to
WHERE e.employee_id IN (SELECT DISTINCT reports_to FROM Employees WHERE reports_to IS NOT NULL)
GROUP BY e.employee_id, e.name
ORDER BY e.employee_id;_  
  
### Q31:  
_SELECT employee_id, 
       CASE 
           WHEN COUNT(DISTINCT department_id) = 1 THEN MAX(department_id)
           ELSE MAX(CASE WHEN primary_flag = 'Y' THEN department_id ELSE NULL END)
       END AS department_id
FROM Employee
GROUP BY employee_id;_  

### Q32:  
_SELECT x, y, z,
    CASE
        WHEN x + y > z AND y + z > x AND x + z > y THEN 'Yes'
        ELSE 'No'
    END AS triangle
FROM Triangle;_   


### Q33:  
_SELECT DISTINCT l1.num AS ConsecutiveNums
FROM Logs l1
JOIN Logs l2 ON l1.id + 1 = l2.id AND l1.num = l2.num
JOIN Logs l3 ON l2.id + 1 = l3.id AND l2.num = l3.num;_  

### Q34:  
_SELECT product_id, 
       COALESCE((
           SELECT new_price 
           FROM Products p2 
           WHERE p1.product_id = p2.product_id 
             AND change_date <= '2019-08-16'
           ORDER BY change_date DESC
           LIMIT 1
       ), 10) AS price
FROM Products p1
GROUP BY product_id;_  


### Q35:  
_SELECT person_name
FROM (
    SELECT 
        person_name,
        SUM(weight) OVER (ORDER BY turn) AS total_weight
    FROM Queue
) AS subquery
WHERE total_weight <= 1000
ORDER BY total_weight DESC
LIMIT 1;_  


### Q36:  
_SELECT 
    categories.category,
    COALESCE(COUNT(account_id), 0) AS accounts_count
FROM (
    SELECT 'Low Salary' AS category
    UNION ALL
    SELECT 'Average Salary'
    UNION ALL
    SELECT 'High Salary'
) AS categories
LEFT JOIN (
    SELECT
        account_id,
        CASE
            WHEN income < 20000 THEN 'Low Salary'
            WHEN income >= 20000 AND income <= 50000 THEN 'Average Salary'
            ELSE 'High Salary'
        END AS category
    FROM Accounts
) AS categorized_accounts
ON categories.category = categorized_accounts.category
GROUP BY categories.category
ORDER BY 
    CASE
        WHEN categories.category = 'Low Salary' THEN 1
        WHEN categories.category = 'Average Salary' THEN 2
        ELSE 3
    END;_  
  
### Q37:  
_SELECT employee_id
FROM Employees
WHERE salary < 30000
AND manager_id NOT IN (SELECT employee_id FROM Employees)
ORDER BY employee_id;_  

  
### Q38:    
_SELECT 
    CASE 
        WHEN id % 2 = 1 AND id != (SELECT MAX(id) FROM Seat) THEN id + 1
        WHEN id % 2 = 0 THEN id - 1
        ELSE id
    END AS id,
    student
FROM 
    Seat
ORDER BY 
    id;_  

### Q39: 
_(
  select name as results 
  from movierating mr inner join users u 
  on mr.user_id=u.user_id
  group by u.user_id 
  order by count(*) desc, name asc 
  limit 1
)
union all
(
  select results from
  (
  select title as results, avg(rating) as average_rating 
  from movierating mr 
  inner join movies m 
  on mr.movie_id=m.movie_id 
  where month(created_at) = 2 
  group by m.movie_id
  ) rating_group
  order by average_rating desc, results asc limit 1
);_  


### Q40: 
Select c.visited_on, sum(t.amount) as amount , ROUND((sum(t.amount)/7),2)as average_amount
from (Select visited_on, sum(amount) as amount
from Customer group by visited_on ) as c, (Select visited_on, sum(amount) as amount
from Customer group by visited_on ) as t
where c.visited_on>=t.visited_on and DATEDIFF(c.visited_on,t.visited_on)<=6
group by c.visited_on having count(distinct t.visited_on)=7  


### Q41:  
_SELECT id, num
FROM (
    SELECT user_id AS id, COUNT(*) AS num
    FROM (
        SELECT requester_id AS user_id FROM RequestAccepted
        UNION ALL
        SELECT accepter_id AS user_id FROM RequestAccepted
    ) AS all_users
    GROUP BY user_id
) AS friend_counts
ORDER BY num DESC
LIMIT 1;_  

### Q42: 
_SELECT ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM Insurance
WHERE tiv_2015 IN (
    SELECT tiv_2015
    FROM Insurance
    GROUP BY tiv_2015
    HAVING COUNT(*) > 1
)
AND (lat, lon) NOT IN (
    SELECT lat, lon
    FROM Insurance
    GROUP BY lat, lon
    HAVING COUNT(*) > 1
);_  

### Q43:  
_SELECT d.name AS Department, e.name AS Employee, e.salary AS Salary
FROM Employee e
JOIN Department d ON e.departmentId = d.id
WHERE (
    SELECT COUNT(DISTINCT salary)
    FROM Employee
    WHERE departmentId = e.departmentId AND salary > e.salary
) < 3
ORDER BY d.name, e.salary DESC;_  

### Q44:   
_SELECT user_id, CONCAT(UCASE(LEFT(name, 1)), LCASE(SUBSTRING(name, 2))) AS name
FROM Users
ORDER BY user_id;_  

### Q45:  
_SELECT patient_id, patient_name, conditions
FROM Patients
WHERE conditions LIKE 'DIAB1%' OR conditions LIKE '% DIAB1%'_  
  
### Q46:   
_DELETE p1
FROM Person p1
JOIN Person p2 ON p1.email = p2.email AND p1.id > p2.id_  
  
### Q47:   
_SELECT
    MAX(salary) AS SecondHighestSalary
FROM
    Employee
WHERE
    salary < (SELECT MAX(salary) FROM Employee)_  

### Q48:   
_SELECT
    sell_date,
    COUNT(DISTINCT product) AS num_sold,
    GROUP_CONCAT(DISTINCT product ORDER BY product) AS products
FROM
    Activities
GROUP BY
    sell_date
ORDER BY
    sell_date;_





### Q49: 
_SELECT
    p.product_name,
    SUM(o.unit) AS unit
FROM
    Products p
    JOIN Orders o ON p.product_id = o.product_id
WHERE
    MONTH(o.order_date) = 2 AND YEAR(o.order_date) = 2020
GROUP BY
    p.product_name
HAVING
    SUM(o.unit) >= 100;_


### Q50:   
_SELECT
    user_id,
    name,
    mail
FROM
    Users
WHERE
    mail REGEXP '^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode\.com$';

