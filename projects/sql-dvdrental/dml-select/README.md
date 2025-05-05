# Part 1: Data Retrieval Using SQL

This section answers analytical questions using `SELECT`, `JOIN`, `WHERE`, and aggregation statements on the [dvdrental](https://www.postgresqltutorial.com/postgresql-sample-database/) dataset.

---

### Q1: List all animation movies released between 2017 and 2019 with a rating higher than 1, ordered alphabetically.

```sql
SELECT f.title,
       f.release_year, 
       f.rental_rate 
FROM public.film f 
LEFT JOIN public.film_category fc ON fc.film_id = f.film_id 
LEFT JOIN public.category c ON fc.category_id = c.category_id 
WHERE f.release_year BETWEEN '2017' AND '2019'
  AND f.rental_rate > 1
  AND upper(c.name) = 'ANIMATION'
ORDER BY f.title;
```
---

### Q2: Calculate the revenue earned by each rental store since March 2017.

```sql
SELECT s.store_id, 
       SUM(p.amount) AS total_payment, 
       CONCAT(a.address, ' ', COALESCE(a.address2, '')) AS store_address
FROM public.payment p
LEFT JOIN public.rental r ON p.rental_id = r.rental_id
LEFT JOIN public.inventory i ON r.inventory_id = i.inventory_id
LEFT JOIN public.store s ON i.store_id = s.store_id
LEFT JOIN public.address a ON s.address_id = a.address_id
WHERE EXTRACT(YEAR FROM p.payment_date) >= 2017 
  AND EXTRACT(MONTH FROM p.payment_date) >= 3
GROUP BY s.store_id, store_address
ORDER BY s.store_id;
```
### Q3: Top-5 actors by number of movies (released since 2015)
```sql
SELECT a.first_name, 
       a.last_name, 
       COUNT(f.film_id) AS number_of_movies 
FROM public.actor a
LEFT JOIN public.film_actor fa ON a.actor_id = fa.actor_id 
LEFT JOIN public.film f ON fa.film_id = f.film_id 
WHERE f.release_year >= '2015'
GROUP BY a.actor_id, a.first_name, a.last_name
ORDER BY number_of_movies DESC
LIMIT 5;
```
### Q4: Number of Drama, Travel, and Documentary movies per year
```sql
SELECT f.release_year,
       COALESCE(SUM(CASE WHEN upper(c.name) = 'DRAMA' THEN 1 ELSE 0 END), 0) AS number_of_drama_movies,
       COALESCE(SUM(CASE WHEN upper(c.name) = 'TRAVEL' THEN 1 ELSE 0 END), 0) AS number_of_travel_movies,
       COALESCE(SUM(CASE WHEN upper(c.name) = 'DOCUMENTARY' THEN 1 ELSE 0 END), 0) AS number_of_documentary_movies
FROM public.film f
LEFT JOIN public.film_category fc ON f.film_id = fc.film_id 
LEFT JOIN public.category c ON fc.category_id = c.category_id 
GROUP BY f.release_year 
ORDER BY f.release_year DESC;
```

### Q5: For each client, list horror movies rented (comma-separated) and total amount paid
```sql
SELECT p.customer_id,
       COALESCE(STRING_AGG(f.title, ', '), ' ') AS horror_movie_titles,
       SUM(p.amount) AS total_amount
FROM public.payment p
LEFT JOIN public.rental r ON p.rental_id = r.rental_id
LEFT JOIN public.inventory i ON r.inventory_id = i.inventory_id
LEFT JOIN public.film f ON i.film_id = f.film_id
LEFT JOIN public.film_category fc ON f.film_id = fc.film_id
LEFT JOIN public.category c ON fc.category_id = c.category_id
WHERE upper(c.name) = 'HORROR'
GROUP BY p.customer_id;
```

# Part 2: Solving problems using SQL
### Q1: Which three employees generated the most revenue in 2017? They should be awarded a bonus for their outstanding performance.

#### Assumptions:
- An employee may have worked in multiple stores throughout the year.
- The store assignment is based on the **most recent transaction** (`payment_date`) processed by the staff.
- Only payments made in **2017** are considered.
- A staff member is assumed to be working at the **store where the payment was processed**.

```sql
select p.staff_id,
		s.first_name, 
		s.last_name, 
		s.store_id, 
		SUM(p.amount) as total_sales --calculate total sales for each employee
from public.payment p 
left join public.staff s on p.staff_id = s.staff_id --join two table to get staff details
where p.payment_date between '2017-01-01' and '2017-12-31' --filter for 2017 year
group by p.staff_id, s.first_name, s.last_name, s.store_id
order by total_sales DESC
limit 3;
```

### Q2: Which 5 movies were rented more than others (number of rentals), and what's the expected age of the audience for these movies? To determine expected age please use 'Motion Picture Association film rating system
```sql
select f.title,
       count(r.rental_id) as times_rented,
       f.rating,
case 
    when f.rating = 'G' then 'General Audience (0+)'  -- General Audiences (0+)
    when f.rating = 'PG' then 'Parental Guidance (10+)'  -- Parental Guidance (10+)
    when f.rating = 'PG-13' then 'Parents Strongly Cautioned (13+)'  -- Parents Strongly Cautioned (13+)
    when f.rating = 'R' then 'Restricted (17+)'  -- Restricted (17+)
    when f.rating = 'NC-17' then 'Adults Only (18+)' -- Adults Only (18+)
    else 'unknown'                       
end as expected_age
       
from public.film f
left join public.inventory i on f.film_id = i.film_id 
left join public.rental r on i.inventory_id = r.inventory_id

-- group by film_id for unique film records
group by f.film_id, f.title, f.rating

-- order by the number of times rented, descending, and limit to top 5
order by times_rented desc
limit 5;
```

# Part 3: Which actors/actresses didn't act for a longer period of time than the others?
#### The task can be interpreted in various ways, and here are a few options:
 - V1: gap between the latest release_year and current year per each actor;
 - V2: gaps between sequential films per each actor;

```sql
--V1
select 
    a.actor_id,
    a.first_name,
    a.last_name,
    max(f.release_year) as latest_release_year,  -- most recent movie release year per actor
    extract(year from current_date) - max(f.release_year) as gap_years  -- calculate gap between current year and latest release year
    
from public.actor a
left join public.film_actor fa on a.actor_id = fa.actor_id  -- join to link actors with films
left join public.film f on fa.film_id = f.film_id           -- join to access film release years

group by a.actor_id, a.first_name, a.last_name

-- order by gap in descending order to get actors with the longest gap first
order by gap_years desc;

--V2:
with actorfilms as (
    -- retrieve actor details and films they acted in
    select 
        a.actor_id,
        a.first_name,
        a.last_name,
        f.title as film_title,
        f.release_year
    from public.actor a
    join public.film_actor fa on a.actor_id = fa.actor_id
    join public.film f on fa.film_id = f.film_id
),
filmgaps as (
    -- calculate gap between current and previous release years per actor
    select 
        af1.actor_id,
        af1.first_name,
        af1.last_name,
        af1.release_year as current_release_year,
        max(af2.release_year) as previous_release_year,
        af1.release_year - max(af2.release_year) as gap_years
    from actorfilms af1
    left join actorfilms af2 
        on af1.actor_id = af2.actor_id 
        and af2.release_year < af1.release_year
    group by 
        af1.actor_id, 
        af1.first_name, 
        af1.last_name, 
        af1.release_year
),
maxgaps as (
    -- get the maximum gap years per actor
    select 
        actor_id,
        first_name,
        last_name,
        max(gap_years) as max_gap_years
    from filmgaps
    group by actor_id, first_name, last_name
)
-- retrieve actors with the longest gap between film releases
select 
    actor_id,
    first_name,
    last_name,
    max_gap_years
from maxgaps
order by max_gap_years desc;
```
