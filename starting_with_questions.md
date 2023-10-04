Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

select distinct a_s.city, a_s.country, a_s.totalTransactionRevenue
from all_sessions a_s
where a_s.totalTransactionRevenue is not null
order by 3 desc


Answer:

"Atlanta"	"United States"	742480000
"Sunnyvale"	"United States"	649240000
"Tel Aviv-Yafo"	"Israel"	602000000
"Los Angeles"	"United States"	363000000
"Seattle"	"United States"	358000000
"Sydney"	"Australia"	358000000
"Chicago"	"United States"	306000000
"Palo Alto"	"United States"	305000000
"San Francisco"	"United States"	301000000
"San Francisco"	"United States"	202000000
"Sunnyvale"	"United States"	200000000
"San Francisco"	"United States"	196090000
"Nashville"	"United States"	157000000
"San Francisco"	"United States"	157000000
"Mountain View"	"United States"	156000000
"Mountain View"	"United States"	154000000
"San Jose"	"United States"	154000000
"New York"	"United States"	152000000
"Palo Alto"	"United States"	152000000
"Palo Alto"	"United States"	151000000
"San Francisco"	"United States"	139420000
"New York"	"United States"	127000000
"San Francisco"	"United States"	124000000
"Chicago"	"United States"	123940000
"San Francisco"	"United States"	123000000
"Austin"	"United States"	122000000
"Sunnyvale"	"United States"	120000000
"Los Angeles"	"United States"	116480000
"San Francisco"	"United States"	115520000
"Atlanta"	"United States"	111960000
"San Jose"	"United States"	108380000
"San Francisco"	"United States"	108140000
"San Bruno"	"United States"	103770000
"Toronto"	"Canada"	82160000
"New York"	"United States"	81960000
"Mountain View"	"United States"	71190000
"New York"	"Canada"	67990000
"San Francisco"	"United States"	61970000
"New York"	"United States"	57990000
"New York"	"United States"	51330000
"Houston"	"United States"	38980000
"Mountain View"	"United States"	35990000
"Austin"	"United States"	35780000
"New York"	"United States"	32990000
"Mountain View"	"United States"	26820000
"San Francisco"	"United States"	23990000
"Sunnyvale"	"United States"	22990000
"Columbus"	"United States"	21990000
"New York"	"United States"	19590000
"Chicago"	"United States"	19580000
"Mountain View"	"United States"	16990000
"Zurich"	"Switzerland"	16990000
"Mountain View"	"United States"	13390000
"San Francisco"	"United States"	12190000
"Mountain View"	"United States"	8980000
"New York"	"United States"	7500000


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

Select distinct country, 
avg(productquantity) over (partition by country)
, city,  avg(productquantity) over (partition by city)
from all_sessions
where country not like '%(not set)'
and productquantity is not null
and city not like '%not available%' 
and city not like '%not set%' 
order by 2, 4 


Answer:

"India"	1.00000000000000000000	"Bengaluru"	1.00000000000000000000
"Ireland"	1.00000000000000000000	"Dublin"	1.00000000000000000000
"United States"	1.4137931034482759	"Salem"	8.0000000000000000
"United States"	1.4137931034482759	"Atlanta"	4.0000000000000000
"United States"	1.4137931034482759	"Houston"	2.0000000000000000
"United States"	1.4137931034482759	"New York"	1.1666666666666667
"United States"	1.4137931034482759	"Ann Arbor"	1.00000000000000000000
"United States"	1.4137931034482759	"Chicago"	1.00000000000000000000
"United States"	1.4137931034482759	"Columbus"	1.00000000000000000000
"United States"	1.4137931034482759	"Dallas"	1.00000000000000000000
"United States"	1.4137931034482759	"Detroit"	1.00000000000000000000
"United States"	1.4137931034482759	"Los Angeles"	1.00000000000000000000
"United States"	1.4137931034482759	"Mountain View"	1.00000000000000000000
"United States"	1.4137931034482759	"Palo Alto"	1.00000000000000000000
"United States"	1.4137931034482759	"San Francisco"	1.00000000000000000000
"United States"	1.4137931034482759	"San Jose"	1.00000000000000000000
"United States"	1.4137931034482759	"Seattle"	1.00000000000000000000
"United States"	1.4137931034482759	"Sunnyvale"	1.00000000000000000000
"Spain"	10.0000000000000000	"Madrid"	10.0000000000000000



**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

select distinct country, city, v2productcategory
from all_sessions
where country not like '%(not set)'
and productquantity is not null
and city not like '%not%' 
and v2productcategory not like '%(not%' 
 and v2productcategory like '%Nest%' 
and country = 'United States'
order by 1, 2


Answer:
Of the 24 US cities, people in 10 cities seem to have some liking for 'Nest' products.




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







