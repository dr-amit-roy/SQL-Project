What issues will you address by cleaning the data?

To find if SalesBySKu had any products that are not in Products table:

This could be because of human error or new products not yet added to Products table. This could be cnfusing so I deleted these rows. These 8 rows amounted to a total of 5 orders out of 5524 total orders. I chose to delete the eight rows assuming them to be mistakes introduced by human error; and deleted them to establish the Foreign Key relationship. The cost of assuming these eight values as human error seemed reasonable as only 0.001% of the total orders were ignored. 

I chose to divide the unit_price by 1000000 in the Analytics table wherever it was used before that I was thinking the online business is making this company ridiculously rich by selling two not so abnormal products.

Queries:

Query to check if the 'productsku' values are correct.

select productsku 
	from salesbysku
	where productsku not in (select sku from products)

Query to delete the rows where productsku value has no matched in Products table:

Delete 
from salesbysku
where productsku in 
( select productsku 
	from salesbysku
	where productsku not in (select sku from products))


