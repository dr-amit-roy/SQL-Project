Question 1: 

What percentage of our products are the online visitors interested in? What do online customers buy? 

SQL Queries:
-- Total number of our Products
select count(*)  from products

--  Products not seen by online visitors
select count(p.sku) from products p
where p.sku not in (select distinct productsku from all_sessions) 

--  Products seen by online visitors
select count(p.sku) from products p
where p.sku in (select distinct productsku from all_sessions) 

--  Related query: Products online customers want to purchase. 
select v2productname, count(*) from all_sessions 
where ecommerceaction_type = 5
group by 1
order by 2 desc
-- 'ecommerceaction_type = 5' indicates user action to purchase a product 


Answers: 
1092

703

389

389/1092 * 100 = 36.6%

"Nest® Learning Thermostat 3rd Gen-USA - Stainless Steel"	5
"Google Sunglasses"	3
"Google Men's  Zip Hoodie"	2
"Google Laptop and Cell Phone Stickers"	2
"Nest® Cam Outdoor Security Camera - USA"	2
"Google Men's Bike Short Sleeve Tee Charcoal"	1
"Google Men's Short Sleeve Badge Tee Charcoal"	1
"Google Men's Vintage Badge Tee Black"	1
"Google Men's Vintage Badge Tee Sage"	1
"Google Men's Vintage Badge Tee White"	1
"Google Men's Vintage Tank"	1
"Google Womens 3/4 Sleeve Baseball Raglan Heather/Black"	1
"Nest® Learning Thermostat 3rd Gen-USA - White"	1
"Red Spiral Google Notebook"	1
"Reusable Shopping Bag"	1
"Waze Mood Original Window Decal"	1
"Android Wool Heather Cap Heather/Black"	1
"YouTube Men's Short Sleeve Hero Tee Charcoal"	1
"Four Color Retractable Pen"	1
"Google 22 oz Water Bottle"	1
"Google Men's 3/4 Sleeve Raglan Henley Grey"	1
"Google Men's Airflow 1/4 Zip Pullover Lapis"	1


Question 2: 
Which products have we been selling online? How many of them?

SQL Queries:
-- Assumption: the word 'sell' is the keyword here. I have assumed through this assignment that a non-null  
-- TransactionId column value in All_Sessions column is the only criteria to find if the purchase has already been made or not.

select distinct name, sum(an.units_sold) as total_sold
from products p 
inner join all_sessions a_s
on p.sku = a_s.productsku
inner join analytics an
on an.visitid = a_s.visitid
where  an.units_sold>0
-- and a_s.transactionid is not null 
and a_s.visit_date = an.date
group by name
order by 2 desc



Answer:
"SPF-15 Slim & Slender Lip Balm"	44
"Mobile Phone Vent Mount"	8


Question 3: 
Should I reorder more stock for our online top-seller products? 

SQL Queries:

select distinct name, p.stocklevel, sum(an.units_sold) as total_sold
from products p 
inner join all_sessions a_s
on p.sku = a_s.productsku
inner join analytics an
on an.visitid = a_s.visitid
where  an.units_sold>0
and a_s.transactionid is not null 
and a_s.visit_date = an.date
group by name, 2
order by 3 desc

Answer:

"SPF-15 Slim & Slender Lip Balm"	4069	44
" Mobile Phone Vent Mount"	129	8

So 'No'. There is no need to reorder the stock just now.

Question 4: 
Which country are our leading online purchasers from? 

SQL Queries:

select distinct country, sum(revenue) /1000000, currencycode
from analytics al
inner join all_sessions a_s
on al.visitid = a_s.visitid
where revenue is not null 
and currencycode = 'USD'
group by country, currencycode
order by 2 desc


Answer:

"United States"	4804.33
"Israel"	32.99
"Switzerland"	16.99


Question 5: 
What online product categories feature our highest  selling offline products?

SQL Queries:

With ThisIsCte as
(
  select name, sum(total_ordered) from salesreport
  where total_ordered >0
  group by name
  order by 2 desc
  Limit 5
)
Select distinct sr.name, a_s.v2productcategory, sr.total_ordered 
from all_sessions a_s
inner join salesreport sr
on a_s.productsku = sr.productsku
inner join ThisIsCte t 
on sr.name= t.name
order by 3 desc;

Answer:

"Ballpoint LED Light Pen"	"(not set)"	456
"Ballpoint LED Light Pen"	"Home/Office/"	456
"Ballpoint LED Light Pen"	"Home/Office/Writing Instruments/"	456
" 17oz Stainless Steel Sport Bottle"	"(not set)"	334
" 17oz Stainless Steel Sport Bottle"	"Home/Accessories/Drinkware/"	334
" 17oz Stainless Steel Sport Bottle"	"Home/Drinkware/"	334
" 17oz Stainless Steel Sport Bottle"	"Home/Drinkware/Water Bottles and Tumblers/"	334
" 17oz Stainless Steel Sport Bottle"	"Home/Shop by Brand/Google/"	334
"Leatherette Journal"	"Home/Office/"	319
"Leatherette Journal"	"Home/Office/Notebooks & Journals/"	319
" Spiral Journal with Pen"	"Home/Office/Notebooks & Journals/"	290
"Foam Can and Bottle Cooler"	"Home/Accessories/Drinkware/"	253
"Foam Can and Bottle Cooler"	"Home/Accessories/Housewares/"	253
"Foam Can and Bottle Cooler"	"Home/Drinkware/"	253
"Foam Can and Bottle Cooler"	"Home/Lifestyle/"	253
"Foam Can and Bottle Cooler"	"Home/Lifestyle/Fun/"	253
"Foam Can and Bottle Cooler"	"(not set)"	0
