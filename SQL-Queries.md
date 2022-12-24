# SQL Queries


## Top 10 Countries
```
WITH top_5_customers AS
(SELECT
	SUM(A.amount) AS total_payments,
	B.customer_id,
	B.first_name,
	B.last_name,
	D.city,
	E.country
FROM payment A
	INNER JOIN customer B ON A.customer_id = B.customer_id
	INNER JOIN address C ON B.address_id = C.address_id
	INNER JOIN city D ON C.city_id = D.city_id
	INNER JOIN country E ON D.country_id = E.country_id
WHERE D.city IN(
	'Aurora',
	'Tokat',
	'Tarsus',
	'Atlixco',
	'Emeishan',
	'Pontianak',
	'Shimoga', 
	'Aparecida de Goinia',
	'Zalantun',
	'Taguig')
GROUP BY B.customer_id, D.city, E.country
ORDER BY SUM(A.amount) DESC
LIMIT 5)
SELECT AVG(total_payments) AS average_top_5_amount_paid
FROM top_5_customers
```

## Top 5 customers 
```
WITH top_5_customers AS
(SELECT
	SUM(A.amount) AS total_payments,
	B.customer_id,
	B.first_name,
	B.last_name,
	D.city,
	E.country
FROM payment A
	INNER JOIN customer B ON A.customer_id = B.customer_id
	INNER JOIN address C ON B.address_id = C.address_id
	INNER JOIN city D ON C.city_id = D.city_id
	INNER JOIN country E ON D.country_id = E.country_id
WHERE D.city IN(
	'Aurora',
	'Tokat',
	'Tarsus',
	'Atlixco',
	'Emeishan',
	'Pontianak',
	'Shimoga', 
	'Aparecida de Goinia',
	'Zalantun',
	'Taguig')
GROUP BY B.customer_id, D.city, E.country
ORDER BY SUM(A.amount) DESC
LIMIT 5)
```

## Country list

```
SELECT 
E.country,
COUNT(B.customer_id) AS all_customer_count, 
COUNT (DISTINCT top_5_customers.country) AS top_customer_count
FROM customer B
		INNER JOIN address C ON B.address_id = C.address_id
		INNER JOIN city D ON C.city_id = D.city_id
		INNER JOIN country E ON D.country_id = E.country_id
		LEFT JOIN top_5_customers ON E.country=top_5_customers.country
GROUP BY E.country
	ORDER BY all_customer_count DESC
```
