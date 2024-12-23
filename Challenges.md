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
#### Day 7
- The owner of a winter market wants to know which vendors have generated the highest revenue overall. For each vendor, calculate the total revenue for all their items and return a list of the top 2 vendors by total revenue. Include the vendor_name and total_revenue in your results.
````sql
select vendor_name, sum(quantity_sold*price_per_unit) as total_revenue
from vendors as v 
inner join sales as s 
on v.vendor_id = s.vendor_id
group by vendor_name
order by total_revenue desc
limit 2
````
#### Day 8
- You are managing inventory in Santa's workshop. Which gifts are meant for "good" recipients? List the gift name and its weight.
````sql
select gift_name, weight_kg
from gifts
where recipient_type = "good"
````
#### Day 9
- A community is hosting a series of festive feasts, and they want to ensure a balanced menu. Write a query to identify the top 3 most calorie-dense dishes (calories per gram) served for each event. Include the dish_name, event_name, and the calculated calorie density in your results.
````sql
with cte as (select dish_name , e.event_name, m.calories/m.weight_g as col1,
ROW_NUMBER() OVER (PARTITION BY e.event_name ORDER BY m.calories/m.weight_g DESC) AS intRow
from events as e
inner join menu as m 
on e.event_id = m.event_id)

select dish_name, event_name, col1
from cte 
where intRow in(1,2,3)
````
#### Day 10
- You are tracking your friends' New Year’s resolution progress. Write a query to calculate the following for each friend: number of resolutions they made, number of resolutions they completed, and success percentage (% of resolutions completed) and a success category based on the success percentage: <br>
Green: If success percentage is greater than 75%. <br>
Yellow: If success percentage is between 50% and 75% (inclusive). <br>
Red: If success percentage is less than 50%.
````sql
SELECT 
    friend_name, 
    COUNT(DISTINCT resolution_id) AS num, 
    SUM(is_completed) AS com, 
    (CAST(SUM(is_completed) AS FLOAT) / COUNT(DISTINCT resolution_id)) * 100 AS pct,
    CASE 
        WHEN (CAST(SUM(is_completed) AS FLOAT) / COUNT(DISTINCT resolution_id)) * 100 < 50 THEN 'Red'
        WHEN (CAST(SUM(is_completed) AS FLOAT) / COUNT(DISTINCT resolution_id)) * 100 BETWEEN 50 AND 75 THEN 'Yellow'
        ELSE 'Green' 
    END AS color
FROM resolutions 
GROUP BY friend_name
````
#### Day 11
- You are preparing holiday gifts for your family. Who in the family_members table are celebrating their birthdays in December 2024? List their name and birthday.
````sql
select name 
from family_members
where birthday between "2024-12-01" and "2024-12-31"
````
#### Day 12 
- A collector wants to identify the top 3 snow globes with the highest number of figurines. Write a query to rank them and include their globe_name, number of figurines, and material.
````sql
with cte as (select globe_name, count(*) as num, material 
from snow_globes as a
inner join figurines as f 
on a.globe_id = f.globe_id
group by globe_name, material
order by num desc)
select *
from cte
limit 3
````
#### Day 13
- We need to make sure Santa's sleigh is properly balanced. Find the total weight of gifts for each recipient.
````sql
select recipient, sum(weight_kg) 
from gifts
group by recipient
````
#### Day 14
- Which ski resorts had snowfall greater than 50 inches?
````sql
select resort_name
from snowfall
where snowfall_inches > 50
````
#### Day 15
- A family reunion is being planned, and the organizer wants to identify the three family members with the most children. Write a query to calculate the total number of children for each parent and rank them. Include the parent’s name and their total number of children in the result.
````sql
select name, count(p.child_id)
from family_members as f  
inner join parent_child_relationships as p 
on f.member_id = p.parent_id
group by p.parent_id
order by count(p.child_id) desc
limit 3
````
#### Day 16
- As the owner of a candy store, you want to understand which of your products are selling best. Write a query to calculate the total revenue generated from each candy category.
````sql
select category, sum(quantity_sold*price_per_unit) as revenue
from candy_sales 
group by category
````
#### Day 17
- The Grinch is planning out his pranks for this holiday season. Which pranks have a difficulty level of “Advanced” or “Expert"? List the prank name and location (both in descending order).
````sql
select prank_name, location
from grinch_pranks
where difficulty in ("Advanced", "Expert") 
order by prank_name desc, location desc
````
#### Day 18
- A travel agency is promoting activities for a "Summer Christmas" party. They want to identify the top 2 activities based on the average rating. Write a query to rank the activities by average rating.
````sql
select activity_name, avg(rating)
from activities as a 
inner join activity_ratings as ar 
on a.activity_id = ar.activity_id
group by activity_name
order by avg(rating) desc
limit 2
````
#### Day 19
- Scientists are studying the diets of polar bears. Write a query to find the maximum amount of food (in kilograms) consumed by each polar bear in a single meal December 2024. Include the bear_name and biggest_meal_kg, and sort the results in descending order of largest meal consumed.
```sql
select bear_name, max(food_weight_kg) as kg
from polar_bears as p 
inner join meal_log as m 
on p.bear_id = m.bear_id
where date between "2024-12-01" and "2024-12-31"
group by bear_name
order by kg desc
````
#### Day 20
- We are looking for cheap gifts at the market. Which vendors are selling items priced below $10? List the unique (i.e. remove duplicates) vendor names.
````sql
select distinct vendor_name
from vendors as v  
inner join item_prices as i 
on v.vendor_id = i.vendor_id
where price_usd < 10
````
#### Day 21
- Santa needs to optimize his sleigh for Christmas deliveries. Write a query to calculate the total weight of gifts for each recipient type (good or naughty) and determine what percentage of the total weight is allocated to each type. Include the recipient_type, total_weight, and weight_percentage in the result.
````sql
select recipient_type, sum(weight_kg), 
round(sum(weight_kg)/(select sum(weight_kg) from gifts)*100,2) as pct
from gifts
group by recipient_type
````
#### Day 22
- We are hosting a gift party and need to ensure every guest receives a gift. Using the guests and guest_gifts tables, write a query to identify the guest(s) who have not been assigned a gift (i.e. they are not listed in the guest_gifts table).
````sql
select guest_name
from guests as g  
left join guest_gifts as gg  
on g.guest_id = gg.guest_id
where gg.guest_id is null
````
#### Day 23
- The Grinch tracked his weight every day in December to analyze how it changed daily. Write a query to return the weight change (in pounds) for each day, calculated as the difference from the previous day's weight.
````sql
SELECT log_id, day_of_month, weight - LAG(weight) OVER (ORDER BY day_of_month) AS weight_change
FROM grinch_weight_log
````
