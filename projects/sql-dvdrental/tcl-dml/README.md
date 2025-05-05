# Data Manipulation Using TCL & DML Statement

This section focuses on modifying data within the [dvdrental](https://www.postgresqltutorial.com/postgresql-sample-database/) dataset using Transaction Control Language (TCL) and Data Manipulation Language (DML). It includes operations such as `INSERT`, `UPDATE`, `DELETE`, and demonstrates the use of transactions to ensure data integrity.

---

### Task: 
- Choose your top-3 favorite movies and add them to the 'film' table 
- Fill in rental rates with 4.99, 9.99 and 19.99 and rental durations with 1, 2 and 3 weeks respectively.
- Add the actors who play leading roles in your favorite movies to the 'actor' and 'film_actor' tables (6 or more actors in total). 
- Add your favorite movies to any store's inventory.
- Alter any existing customer in the database with at least 43 rental and 43 payment records. Change their personal data to yours (first name, last name, address, etc.). You can use any existing address from the "address" table. 
- Remove any records related to you (as a customer) from all tables except 'Customer' and 'Inventory'
- Rent you favorite movies from the store they are in and pay for them (add corresponding records to the database to represent this activity)

```sql
-- Q1 & Q2. Insert favorite movies with rental rates, durations, and last update

insert into public.film (title, rental_rate, rental_duration, release_year, language_id, last_update)
select 'The Godfather', 4.99, 7, 1972,
		(select language_id from public.language where UPPER(name) = 'ENGLISH' limit 1), 
		current_date
where not exists (
    select 1 from public.film where UPPER(title) = 'THE GODFATHER' and release_year = 1972
)
returning film_id;

insert into public.film (title, rental_rate, rental_duration, release_year, language_id, last_update)
select 'Coach Carter', 9.99, 14, 2005,
		(select language_id from public.language where UPPER(name) = 'ENGLISH' limit 1),
		current_date
where not exists (
    select 1 from public.film where upper(title) = 'COACH CARTER' and release_year = 2005
)
returning film_id;

insert into public.film (title, rental_rate, rental_duration, release_year, language_id, last_update)
select 'The Pursuit of Happyness', 19.99, 21, 2006, 
		(select language_id from public.language where UPPER(name) = 'ENGLISH' limit 1),
		current_date
where not exists (
    select 1 from public.film where upper(title) = 'THE PURSUIT OF HAPPYNESS' and release_year = 2006
)
returning film_id;

commit;




--Q3. Add the actors who play leading roles in your favorite movies to the 'actor' and 'film_actor' tables (6 or more actors in total). 
-- Actors with the name Actor1, Actor2, etc - will not be taken into account and grade will be reduced.    

--Option 1: hard coding
/*insert into actor (first_name, last_name)
values 
    ('Al', 'Pacino'),              -- The Godfather
    ('Marlon', 'Brando'),          -- The Godfather
    ('Samuel', 'Jackson'),         -- Coach Carter
    ('Channing', 'Tatum'),         -- Coach Carter
    ('Will', 'Smith'),             -- The Pursuit of Happyness
    ('Jaden', 'Smith');            -- The Pursuit of Happyness
    
--insert into film_actor (actor_id, film_id)
--values 
    (201, 1001), (202, 1001),   -- Linking Al Pacino and Marlon Brando to The Godfather
    (203, 1002), (204, 1002),   -- Linking Samuel L. Jackson and Channing Tatum to Coach Carter
    (205, 1003), (206, 1003);   -- Linking Will Smith and Jaden Smith to The Pursuit of Happyness
  */
    
    
    

--Option 2: no hard coding, making queries reusable with NOT EXIST     
    -- Insert actors
insert into public.actor (first_name, last_name, last_update)
select 'Al', 'Pacino', current_date
where not exists (
    select 1 from public.actor where upper(first_name) = 'AL' and upper(last_name) = 'PACINO'
)
returning actor_id;

-- repeat for all other actors
insert into public.actor (first_name, last_name, last_update)
select 'Marlon', 'Brando', current_date
where not exists (
    select 1 from public.actor where upper(first_name) = 'MARLON' and upper(last_name) = 'BRANDO'
)
returning actor_id;

insert into public.actor (first_name, last_name, last_update)
select 'Samuel', 'Jackson', current_date
where not exists (
    select 1 from public.actor where upper(first_name) = 'SAMUEL' and upper(last_name) = 'JACKSON'
)
returning actor_id;

insert into public.actor (first_name, last_name, last_update)
select 'Channing', 'Tatum', current_date
where not exists (
    select 1 from public.actor where upper(first_name) = 'CHANNING' and upper(last_name) = 'TATUM'
)
returning actor_id;

insert into public.actor (first_name, last_name, last_update)
select 'Will', 'Smith', current_date
where not exists (
    select 1 from public.actor where upper(first_name) = 'WILL' and upper(last_name) = 'SMITH'
)
returning actor_id;

insert into public.actor (first_name, last_name, last_update)
select 'Jaden', 'Smith', current_date
where not exists (
    select 1 from public.actor where upper(first_name) = 'JADEN' and upper(last_name) = 'SMITH'
)
returning actor_id;




--Linking actors with films in film_actor table


-- Linking Al Pacino to The Godfather
insert into public.film_actor (actor_id, film_id, last_update)
	select 
		(select actor_id from public.actor where upper(first_name) = 'AL' and upper(last_name) = 'PACINO' limit 1),
		(select film_id from public.film where upper(title) = 'THE GODFATHER'),
		current_date
where not exists (
    select 1 from public.film_actor
    where
    actor_id = (select actor_id from public.actor where upper(first_name) = 'AL' and upper(last_name) = 'PACINO' limit 1)
    and film_id = (select film_id from public.film where upper(title) = 'THE GODFATHER')
);

-- Linking Marlon Brando to The Godfather
insert into public.film_actor (actor_id, film_id, last_update)
	select 
		(select actor_id from public.actor where upper(first_name) = 'MARLON' and upper(last_name) = 'BRANDO' limit 1),
		(select film_id from public.film where upper(title) = 'THE GODFATHER'),
		current_date
where not exists (
    select 1 from public.film_actor
    where
    actor_id = (select actor_id from public.actor where upper(first_name) = 'MARLON' and upper(last_name) = 'BRANDO' limit 1)
    and film_id = (select film_id from public.film where upper(title) = 'THE GODFATHER')
);

-- Linking Samuel L. Jackson to Coach Carter
insert into public.film_actor (actor_id, film_id, last_update)
	select 
		(select actor_id from public.actor where upper(first_name) = 'SAMUEL' and upper(last_name) = 'JACKSON' limit 1),
		(select film_id from public.film where upper(title) = 'COACH CARTER'),
		current_date
where not exists (
    select 1 from public.film_actor
    where
    actor_id = (select actor_id from public.actor where upper(first_name) = 'SAMUEL' and upper(last_name) = 'JACKSON' limit 1)
    and film_id = (select film_id from public.film where upper(title) = 'COACH CARTER')
);

-- Linking Channing Tatum to Coach Carter
insert into public.film_actor (actor_id, film_id, last_update)
	select 
		(select actor_id from public.actor where upper(first_name) = 'CHANNING' and upper(last_name) = 'TATUM' limit 1),
		(select film_id from public.film where upper(title) = 'COACH CARTER'),
		current_date
where not exists (
    select 1 from public.film_actor
    where
    actor_id = (select actor_id from public.actor where upper(first_name) = 'CHANNING' and upper(last_name) = 'TATUM' limit 1)
    and film_id = (select film_id from public.film where upper(title) = 'COACH CARTER')
);

-- Linking Will Smith to The Pursuit of Happyness
insert into public.film_actor (actor_id, film_id, last_update)
	select 
		(select actor_id from public.actor where upper(first_name) = 'WILL' and upper(last_name) = 'SMITH' limit 1),
		(select film_id from public.film where upper(title) = 'THE PURSUIT OF HAPPYNESS'),
		current_date
where not exists (
    select 1 from public.film_actor
    where
    actor_id = (select actor_id from public.actor where upper(first_name) = 'WILL' and upper(last_name) = 'SMITH' limit 1)
    and film_id = (select film_id from public.film where upper(title) = 'THE PURSUIT OF HAPPYNESS')
);

-- Linking Jaden Smith to The Pursuit of Happyness
insert into public.film_actor (actor_id, film_id, last_update)
	select 
		(select actor_id from public.actor where upper(first_name) = 'JADEN' and upper(last_name) = 'SMITH' limit 1),
		(select film_id from public.film where upper(title) = 'THE PURSUIT OF HAPPYNESS'),
		current_date
where not exists (
    select 1 from public.film_actor
    where
    actor_id = (select actor_id from public.actor where upper(first_name) = 'JADEN' and upper(last_name) = 'SMITH' limit 1)
    and film_id = (select film_id from public.film where upper(title) = 'THE PURSUIT OF HAPPYNESS')
);


commit; 


-- Q4: Add your favorite movies to any store's inventory.

--Option 1: with hard coding
/*insert into public.inventory (film_id, store_id)
values 
    (1001, 1),  -- The Godfather
    (1002, 1),  -- Coach Carter
    (1003, 1);  -- The Pursuit of Happyness
    */
    
--Option 2: no hard coding 

--Adding Godfather to random store's inventory 
insert into public.inventory (film_id, store_id, last_update)
select 
    f.film_id,
    (select store_id from public.store limit 1), -- selecting store_id at random
    current_date
from public.film f
where upper(f.title) = 'THE GODFATHER'
and not exists (
    select 1 from public.inventory i
    where i.film_id = f.film_id 
      and i.store_id = (select store_id from public.store limit 1) 
);


-- Adding Coach Carter to random store's inventory
insert into public.inventory (film_id, store_id, last_update)
select 
    f.film_id,
    (select store_id from public.store order by random() limit 1), -- selecting store_id at random
    current_date
from public.film f
where upper(f.title) = 'COACH CARTER'
and not exists (
    select 1 from public.inventory i
    where i.film_id = f.film_id 
      and i.store_id = (select store_id from public.store order by random() limit 1) 
);

-- Adding The Pursuit of Happyness to random store's inventory
insert into public.inventory (film_id, store_id, last_update)
select 
    f.film_id,
    (select store_id from public.store order by random() limit 1), -- selecting store_id at random
    current_date
from public.film f
where upper(f.title) = 'THE PURSUIT OF HAPPYNESS'
and not exists (
    select 1 from public.inventory i
    where i.film_id = f.film_id 
      and i.store_id = (select store_id from public.store order by random() limit 1) 
);


commit; 


--Q5: Altering customer with at least 43 payments
--Option 1: hard coding

 --Step 1: Finding customer with at least 43 rental and 43 payment records
/*
select c.customer_id, c.first_name, c.last_name, c.email, 
       count(distinct r.rental_id) as rental_count, 
       count(distinct p.payment_id) as payment_count
from public.customer c
left join public.rental r on c.customer_id = r.customer_id
left join public.payment p on c.customer_id = p.customer_id
group by c.customer_id, c.first_name, c.last_name
having count(distinct r.rental_id) >= 43 
   and count(distinct p.payment_id) >= 43;
  
--Step 2: alter customer with ID 1 
--update public.customer
--set first_name = UPPER('Arslan'),
    last_name = UPPER('Adilkhan'),
    email = 'arsadilkhan@gmail.com',
    address_id = 1  -- select any existing address
where customer_id = 1;  
*/

--Option 2: no hard coding

update public.customer 
	set first_name = 'ARSLAN',
	last_name = 'ADILKHAN',
	email = 'arsadilkhan@gmail.com',
	address_id = (
		select address_id from public.address
		limit 1),   -- using subquery and limit 1 to select a random id from the address table
	last_update = current_date 
where customer_id = (
		select c.customer_id
    	from public.customer c
    	left join public.rental r on c.customer_id = r.customer_id
    	left join public.payment p on c.customer_id = p.customer_id
    	group by c.customer_id
    	having count(distinct r.rental_id) >= 43 and count(distinct p.payment_id) >= 43
    	limit 1
		)           --used subquery to find 1 customer with more than 43 rentals and payments
returning customer_id, first_name, last_name, email, address_id, last_update;

commit; 


--Q6: Remove any records related to you (as a customer) from all tables except 'Customer' and 'Inventory'

--deleting related records from table "Payment"
delete from public.payment
where customer_id = (
    select customer_id 
    from public.customer
    where upper(first_name) = 'ARSLAN' and upper(last_name) = 'ADILKHAN' and email = 'arsadilkhan@gmail.com'
)
returning payment_id, customer_id, amount, payment_date;

--deleting related records from table "Rental"
delete from public.rental
where customer_id = (
    select customer_id 
    from public.customer
    where first_name = UPPER('Arslan') and last_name = UPPER('Adilkhan') and email = 'arsadilkhan@gmail.com'
)
returning rental_id, customer_id, rental_date, return_date;

commit; 



--Q7: Rent you favorite movies from the store they are in and pay for them (add corresponding records to the database to represent this activity)
--(Note: to insert the payment_date into the table payment, you can create a new partition (see the scripts to install the training database ) or add records for the
-- first half of 2017)

-- Inserting rental records for The Godfather
insert into public.rental (rental_date, inventory_id, customer_id, return_date, staff_id, last_update)
select 
	'2017-01-01',
	(select inventory_id from public.inventory i
	where film_id = (select film_id from public.film f
					 where upper(title) = 'THE GODFATHER')
	),
	(select customer_id from public.customer c
	where upper(first_name) = 'ARSLAN' and upper(last_name) = 'ADILKHAN'),
	'2017-01-08',
	(select staff_id from public.staff
	limit 1),
	current_date
where not exists (
	select 1 from public.rental r 
	where r.inventory_id = (select i.inventory_id 
							from public.inventory i 
							where i.film_id = 
							(	select f.film_id from public.film f
								where upper(title) = 'THE GODFATHER'
								)
							)

	and r.customer_id = (select c.customer_id 
						from public.customer c 
						where upper(first_name) = 'ARSLAN' and upper(last_name) = 'ADILKHAN')
)
returning rental_id, rental_date, inventory_id, customer_id, staff_id, last_update; 
	

-- Inserting rental records for Coach Carter
insert into public.rental (rental_date, inventory_id, customer_id, return_date, staff_id, last_update)
select 
	'2017-01-02',
	(select inventory_id from public.inventory i
	where film_id = (select film_id from public.film f
					 where upper(title) = 'COACH CARTER')
	),
	(select customer_id from public.customer c
	where upper(first_name) = 'ARSLAN' and upper(last_name) = 'ADILKHAN'),
	'2017-01-09',
	(select staff_id from public.staff
	limit 1),
	current_date
where not exists (
	select 1 from public.rental r 
	where r.inventory_id = (select i.inventory_id 
							from public.inventory i 
							where i.film_id = 
							(	select f.film_id from public.film f
								where upper(title) = 'COACH CARTER'
								)
							)

	and r.customer_id = (select c.customer_id 
						from public.customer c 
						where upper(first_name) = 'ARSLAN' and upper(last_name) = 'ADILKHAN')
)
returning rental_id, rental_date, inventory_id, customer_id, staff_id, last_update; 


-- Inserting rental records for The Pursuit of Happyness
insert into public.rental (rental_date, inventory_id, customer_id, return_date, staff_id, last_update)
select 
	'2017-01-03',
	(select inventory_id from public.inventory i
	where film_id = (select film_id from public.film f
					 where upper(title) = 'THE PURSUIT OF HAPPYNESS')
	),
	(select customer_id from public.customer c
	where upper(first_name) = 'ARSLAN' and upper(last_name) = 'ADILKHAN'),
	'2017-01-10',
	(select staff_id from public.staff
	limit 1),
	current_date
where not exists (
	select 1 from public.rental r 
	where r.inventory_id = (select i.inventory_id 
							from public.inventory i 
							where i.film_id = 
							(	select f.film_id from public.film f
								where upper(title) = 'THE PURSUIT OF HAPPYNESS'
								)
							)

	and r.customer_id = (select c.customer_id 
						from public.customer c 
						where upper(first_name) = 'ARSLAN' and upper(last_name) = 'ADILKHAN')
)
returning rental_id, rental_date, inventory_id, customer_id, staff_id, last_update; 


-- Insert payment records for The Godfather rental
insert into public.payment (customer_id, staff_id, rental_id, amount, payment_date)
select r.customer_id, r.staff_id, r.rental_id, 4.99, '2017-01-02'
from public.rental r 
left join public.inventory i on r.inventory_id = i.inventory_id 
left join public.film f on i.film_id = f.film_id
where upper(f.title) = 'THE GODFATHER'
and r.customer_id = (select customer_id from public.customer
					where upper(first_name) = 'ARSLAN' and upper(last_name) = 'ADILKHAN')	
and not exists (
	select 1 from public.payment p 
	where p.rental_id = r.rental_id
)
returning payment_id, customer_id, rental_id, amount, payment_date;


-- Insert payment records for Coach Carter
insert into public.payment (customer_id, staff_id, rental_id, amount, payment_date)
select r.customer_id, r.staff_id, r.rental_id, 9.99, '2017-02-01'
from public.rental r 
left join public.inventory i on r.inventory_id = i.inventory_id 
left join public.film f on i.film_id = f.film_id
where upper(f.title) = 'COACH CARTER'
and r.customer_id = (select customer_id from public.customer
					where upper(first_name) = 'ARSLAN' and upper(last_name) = 'ADILKHAN')	
and not exists (
	select 1 from public.payment p 
	where p.rental_id = r.rental_id
)
returning payment_id, customer_id, rental_id, amount, payment_date;


-- Insert payment records for The Pursuit of Happyness 
insert into public.payment (customer_id, staff_id, rental_id, amount, payment_date)
select r.customer_id, r.staff_id, r.rental_id, 19.99, '2017-03-01'
from public.rental r 
left join public.inventory i on r.inventory_id = i.inventory_id 
left join public.film f on i.film_id = f.film_id
where upper(f.title) = 'THE PURSUIT OF HAPPYNESS'
and r.customer_id = (select customer_id from public.customer
					where upper(first_name) = 'ARSLAN' and upper(last_name) = 'ADILKHAN')	
and not exists (
	select 1 from public.payment p 
	where p.rental_id = r.rental_id
)
returning payment_id, customer_id, rental_id, amount, payment_date;

commit;

```




 
