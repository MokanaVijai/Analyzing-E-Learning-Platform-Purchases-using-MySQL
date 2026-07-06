# Analyzing-E-Learning-Platform-Purchases-using-MySQL

“Analyzing E-Learning Platform Purchases using MySQL”
Tasks
1.	Database and table creation




create database ELearning_Platform;
use ELearning_Platform;


create table learners(
		learner_id int Primary Key,
		full_name varchar(50),
		Country varchar(50));

create table courses(
		course_id int Primary Key,
		course_name varchar(100),
        category varchar(50),
       unit_price bigint
        );

create table purchases(
purchase_id int Primary Key, 
learner_id int,
course_id int, 
Quantity bigint, 
purchase_date date,
      Foreign Key(learner_id) references learners(learner_id),
		Foreign Key(course_id) references courses(course_id);
        		); 


•	Table insertion:
o	5 learners, 5 course, 6, purchases
insert into learners(learner_id,full_name , Country)
values('1','mokana','india'),
	('2', 'karthik','singapore'),
    ('3', 'gowtham', 'china'),
    ('4','monika', 'US'),
    ('5','rajan', 'france');
 

 insert into courses (course_id, course_name, category, unit_price)
values('30','SQL for Beginners', 'Database', 2500),
('31','Python Data Analysis', 'Programming', 4500),
('32','Power BI Dashboard', 'Analytics', 4000),
('33','Machine Learning Basics', 'AI', 6000),
('34','Excel Advanced', 'Productivity', 2000);

insert into purchases(purchase_id,learner_id,course_id,quantity,purchase_date)
values(40, 1, 30, 1, '2026-05-01'),
(41, 2, 31, 4, '2026-05-02'),
(42, 3, 32, 15, '2026-05-03'),
(43, 4, 33, 3, '2026-05-04'),
(44, 5, 34, 11, '2026-05-04'),
(45, 5, 30, 2, '2026-05-05'),
(46, 2, 31, 9, '2026-05-06')


 
Use SQL INNER JOIN, LEFT JOIN, and RIGHT JOIN to:


select l.learner_id,l.full_name, c.course_name, c.category, p.quantity,p.purchase_date ,  c.unit_price , (quantity *unit_price) as total_amount
from learners l left join purchases p
on l.learner_id = p.learner_id 
join courses c	
on c.course_id = p.course_id;
 

3. Analytical Queries
Q1. Display each learner’s total spending (quantity × unit_price) along with their country.
select l.learner_id, l.full_name, l.country,sum(p.quantity * c.unit_price) as total_spending 
from learners l join purchases p 
on l.learner_id = p.learner_id 
join 
courses c 
on c.course_id = p.course_id
group by l.learner_id;
 
Q2. Find the top 3 most purchased courses based on total quantity sold.
select c.course_name, sum(p.quantity) as total_quantity 
from courses c join purchases p
on c.course_id = p.course_id 
 group by c.course_id order by total_quantity desc limit 3;
 
Q3. Show each course category’s total revenue and the number of unique learners who purchased from that category.
select count(distinct l.learner_id),c.category, sum(c.unit_price*p.Quantity) as total_revenue 
 from learners l
 join purchases p 
 on l.learner_id = p.learner_id 
 join courses c 
 on c.course_id = p.course_id 
 group by c.category ;
 
Q4. List all learners who have purchased courses from more than one category.
select l.learner_id, l.full_name,
count(distinct c.category) as total_categories
from learners l join purchases p
on l.learner_id = p.learner_id
join courses c
on c.course_id = p.course_id
group by l.learner_id, l.full_name
having count(distinct c.category) > 1;
 
Q5. Identify courses that have not been purchased at all.
select course_name from courses where course_id not in (select course_id from purchases );
 


Analyzing E-Learning Platform Purchases using MySQL
Project Overview
This project analyzes learner purchases in an e-learning platform using MySQL. The database contains learner details, course information, and purchase transactions. SQL joins and analytical queries were used to generate business insights regarding revenue, learner behavior, and course performance.

Key Insights
1. Top Performing Courses
•	Courses with high purchase quantities generated the highest revenue. 
•	“Python Data Analysis” was among the most purchased courses. 

2. Revenue Analysis
•	Revenue was calculated using: quantity × unit_price
•	Python Data Analysis” course contributed high revenue because it was purchased multiple times and in larger quantities.
•	 Programming and Analytics performed strongly due to consistent learner demand.

3. Learner Purchase Behavior
•	Certain learners purchased courses from multiple categories, showing broader learning interests. 
•	This indicates opportunities for cross-selling related courses. 

4. Category Performance
•	Course categories such as Programming and Analytics showed strong engagement. 
•	Categories with lower purchase counts may require additional promotion. 

5. Unpurchased Courses
•	Some courses were never purchased. 
•	This may indicate low visibility, pricing concerns, or reduced learner demand. 


Recommendations
• Focus Marketing on High-Performing Categories
Promote top-selling categories like Programming and Analytics through targeted campaigns.
• Improve Visibility of Low-Performing Courses
Offer discounts, bundles, or advertisements for courses with low or zero purchases.
• Encourage Cross-Category Learning
Recommend related courses to learners who already purchased from one category.
• Analyze Pricing Strategy
Compare pricing and demand to identify the optimal course pricing structure.
• Track Learner Preferences
Use purchase history to personalize future course recommendations.


Conclusion
The project successfully demonstrated how SQL can be used to analyze e-learning platform data. Using joins, aggregation, grouping, and sub queries helped identify revenue trends, learner behavior, and course performance, supporting better business decision-making.


