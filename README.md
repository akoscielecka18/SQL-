# SQL- Advent Calendar 2024
#### Day 1
- A ski resort company want to know which customers rented ski equipment for more than one type of activity (e.g., skiing and snowboarding). List the customer names and the number of distinct activities they rented equipment for.
  
select customer_name, count(*) <br> from rentals <br> group by customer_name <br> having count( distinct activity) >1
#### Day 2
- Santa wants to know which gifts weigh more than 1 kg. Can you list them?

select gift_name <br> from gifts <br> where weight_kg > 1

