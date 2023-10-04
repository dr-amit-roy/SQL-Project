What are your risk areas? Identify and describe them.

Data Validation
Products table: SKU for PK
Imported data with loosely matching datatypes

Checks were made for :
Missing values
Duplicates
 Date columns in int format – kept intact – max, min, total distinct values, null values tested.
 Checks for links between all_sessions and analytics tables



QA Process:
Describe your QA process and include the SQL queries used to execute it.

-- creates table salesreport

create table SalesReport
(
productSKU		varchar, -- Product Key
total_ordered		int,	-- total orders placed
name			varchar, -- product name
stockLevel		int, -- inventory
restockingLeadTime  	int, -- time needed to refill inventory
sentimentScore		float, -- people's response to product
sentimentMagnitude	float, -- strength of emotion
ratio			float -- market ratio!
)

-- creates table salesbysku 
create table SalesBySKU
(
ProductSKU     varchar,	-- Product Key 
total_ordered  int
)

-- creates table Products
create table Products
(
SKU	varchar,	-Key
name	varchar,
orderedQuantity	int,
stockLevel	int,
restockingLeadTime	int,
sentimentScore		float,
sentimentMagnitude	float
)

-- creates table Analytics
create table Analytics
(
visitNumber	int,
visitId		numeric, -- Exponetial 
visitStartTime	numeric, -- Exponetial
date		int, -- need to convert to date
fullvisitorId	numeric, -- Exponetial
userid		varchar,
channelGrouping		varchar,
socialEngagementType	varchar,
units_sold		int, 
pageviews		int,
timeonsite		int, -- initially was time
bounces			int,
revenue			float,
unit_price		int
)

-- creates table all_sessions
create table ALL_SESSIONS
(
fullVisitorId	 	numeric, -- Exponential
channelGrouping		varchar,
visit_time			int, -- changed col name from time to visit_time
country			varchar,
city			varchar,
totalTransactionRevenue	int,
transactions		int,
timeOnSite		int,
pageviews		int,
sessionQualityDim	int,
visit_date			int, -- changed col name from dete to visit_date
visitId			int,
visit_type			varchar -- changed col name from type to visit_type
productRefundAmount	float,
productQuantity		int,
productPrice		float,
productRevenue		float,
productSKU		varchar,
v2ProductName		varchar,
v2ProductCategory	varchar,
productVariant		varchar,
currencyCode		varchar,	
itemQuantity		float,
itemRevenue		float,
transactionRevenue	float,
transactionId		varchar,
pageTitle		varchar,
searchKeyword		varchar,
pagePathLevel1		varchar,
eCommerceAction_type	int,
eCommerceAction_step	int,
eCommerceAction_option	varchar
)

-- image of tables while doing QA 
-- the text and numbers mentioned on some of the columns were counted for through various queries. 

-- Only some of those queries are listed below to describe the QA process. 

![QA-OldSchool](https://github.com/dr-amit-roy/SQL-Project/assets/139013237/6c13317f-04ee-4639-a854-45cb89d02b60)


-- total rows in the table
select distinct count(*) 
from products

-- pk test1: total unique column values in the table
select distinct count(SKU) 
from products

-- pk test2:  total rows in the table where key IS NULL
select distinct count(SKU)
from products
where sku is null;

-- total rows in the table
select distinct count(*) 
from salesreport

-- total rows in the table
select distinct count(productSKU) 
from salesreport

-- total distinct rows in the table
select distinct count(productSKU) 
from salesreport
where productSKU is null;


-- Foreign key eligibility test
-- Expected Value: '0'
select count(productsku)
from salesreport 
where productsku not in (select sku from products);

--  test for pk
select count(distinct productSKU) 
from salesbysku
-- 462

-- test for fk
select sum(total_ordered)
from salesbysku
where productsku not in (select productsku from salesreport);

select sum(total_ordered)
from salesbysku
where productsku in (select productsku from salesreport);

-- total column values and total rows in table
select count(*) 
from all_sessions;
-- 15134

-- total column values and total rows in table
select distinct count(productSKU) 
from all_sessions;
-- 15134

-- total column values and total rows in table
select distinct count(SKU) 
from products;
--1092

-- total rows where column values don't match 
select distinct count(productSKU)
from all_sessions
where productsku not in (select sku from products);

-- total column values and total rows in table
select * from all_sessions;

-- total column values and total rows in table
select count(visit_type)
from all_sessions
where visit_type = 'EVENT' -- 'PAGE';

-- total column values and total rows in table
select distinct  count(ecommerceaction_option) --, -- _step -- ecommerceaction_type --,, ecommerceaction_option
from all_sessions
where ecommerceaction_option = 'Payment' or ecommerceaction_option = 'Billing and Shipping'
order by 1;

select distinct  ecommerceaction_option --, -- _step -- ecommerceaction_type --,, ecommerceaction_option
from all_sessions;

-- total column values and total rows in table
select distinct  ecommerceaction_option, ecommerceaction_type, ecommerceaction_step
from all_sessions
order by 1,2,3;

-- total column values and total rows in table
select distinct count(fullvisitorid) 
from all_sessions;

-- total column values and total rows in table
select count(*) 
from products
where sku not in (select productsku from all_sessions);

-- total column values and total rows in table
select max(length(cast(fullvisitorid as varchar))), 
	   min(length(cast(fullvisitorid as varchar)))
from all_sessions;

-- trying to link the keys
select count(*) 
from all_sessions 
where fullvisitorid not in ( select fullvisitorid from analytics);


-- column unique value count
select distinct count(fullvisitorid)
from analytics;

-- column unique value count
select distinct count(visitid)
from all_sessions

-- column unique value count
select distinct visitid
from all_sessions;


-- column unique value count
select count(fullvisitorid) from all_sessions;

-- to read values
select distinct v2productname from all_sessions
order by 1
limit 100;

-- matching values to find difference
select distinct count(*) 
from products
where SKU not in (select distinct productsku from all_sessions);

-- matching values in two tables
select distinct count(productsku) 
from all_sessions
where productSKU not in (select distinct SKU 
from products);

-- unique ids
select distinct count(visitid)
from analytics;
-- 430122

-- total unique ids
select distinct count(visitid)
from all_sessions;
-- 15134

-- matching columns in two tables
select distinct count(visitid)
from all_sessions
where visitid not in 
(   select distinct count(visitid)
	from analytics
)
-- Both columns have same names but their dates don't match at all.
