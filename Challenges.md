#### Day 1 
- A ski resort company want to know which customers rented ski equipment for more than one type of activity (e.g., skiing and snowboarding). List the customer names and the number of distinct activities they rented equipment for.
 ````sql
 select customer_name, count(*)
from rentals 
group by customer_name
having count( distinct activity) >1
````
#### Day 2
- Santa wants to know which gifts weigh more than 1 kg. Can you list them?
 ````sql
select gift_name
from gifts
where weight_kg > 1
````
#### Day 3
- You’re trying to identify the most calorie-packed candies to avoid during your holiday binge. Write a query to rank candies based on their calorie count within each category. Include the candy name, category, calories, and rank (rank_in_category) within the category
 ````sql
select candy_name, candy_category, calories,
dense_rank() over(partition by candy_category order by calories desc) as ranking
from candy_nutrition
````
#### Day 4
- You’re planning your next ski vacation and want to find the best regions with heavy snowfall. Given the tables resorts and snowfall, find the average snowfall for each region and sort the regions in descending order of average snowfall. Return the columns region and average_snowfall.
 ````sql
select region, avg(snowfall_inches) as average_snowfall
from ski_resorts as ski
inner join snowfall as snow 
on ski.resort_id = snow.resort_id
group by region
order by average_snowfall
````
#### Day 5
- This year, we're celebrating Christmas in the Southern Hemisphere! Which beaches are expected to have temperatures above 30°C on Christmas Day?
````sql
select beach_name
from beach_temperature_predictions
where expected_temperature_c > 30 and date = "2024-12-25"
````
#### Day 6
- Scientists are tracking polar bears across the Arctic to monitor their migration patterns and caloric intake. Write a query to find the top 3 polar bears that have traveled the longest total distance in December 2024. Include their bear_id, bear_name, and total_distance_traveled in the results.
````sql
select t.bear_id as bear_id, bear_name, sum(distance_km) as total_distance_traveled
from polar_bears as p
inner join tracking as t
on p.bear_id = t.bear_id
where t.date between "2024-12-01" and "2024-12-31"
group by t.bear_id
order by sum(distance_km) desc
limit 3
````
#### 7
- The owner of a winter market wants to know which vendors have generated the highest revenue overall. For each vendor, calculate the total revenue for all their items and return a list of the top 2 vendors by total revenue. Include the vendor_name and total_revenue in your results.
````r
select vendor_name, sum(quantity_sold*price_per_unit) as total_revenue
from vendors as v 
inner join sales as s 
on v.vendor_id = s.vendor_id
group by vendor_name
order by total_revenue desc
limit 2
````

