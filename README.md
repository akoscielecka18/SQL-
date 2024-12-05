# SQL- Advent Calendar 2024
#### Day 1
- A ski resort company want to know which customers rented ski equipment for more than one type of activity (e.g., skiing and snowboarding). List the customer names and the number of distinct activities they rented equipment for.
  
select customer_name, count(*) <br> from rentals <br> group by customer_name <br> having count( distinct activity) >1
#### Day 2
- Santa wants to know which gifts weigh more than 1 kg. Can you list them?

select gift_name <br> from gifts <br> where weight_kg > 1
#### Day 3
- You’re trying to identify the most calorie-packed candies to avoid during your holiday binge. Write a query to rank candies based on their calorie count within each category. Include the candy name, category, calories, and rank (rank_in_category) within the category.

select candy_name, candy_category, calories, <br>
dense_rank() over(partition by candy_category order by calories desc) as ranking <br>
from candy_nutrition

#### Day 4
- You’re planning your next ski vacation and want to find the best regions with heavy snowfall. Given the tables resorts and snowfall, find the average snowfall for each region and sort the regions in descending order of average snowfall. Return the columns region and average_snowfall.
  
select region, avg(snowfall_inches) as average_snowfall <br>
from ski_resorts as ski <br>
inner join snowfall as snow <br>
on ski.resort_id = snow.resort_id <br>
group by region <br>
order by average_snowfall <br>







