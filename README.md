# SQL- Advent Calendar 2024
#### Day 1
- A ski resort company want to know which customers rented ski equipment for more than one type of activity (e.g., skiing and snowboarding). List the customer names and the number of distinct activities they rented equipment for.
  
select customer_name, count(*) <br> from rentals <br> group by customer_name <br> having count( distinct activity) >1

