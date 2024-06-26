Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.


### Решение 1
[more_300](https://github.com/sash3939/SQL-part2/assets/156709540/233a93c9-c013-426e-b21e-47b2827d0a8d)


select
	concat(st.last_name, st.first_name) as staff_name, ci.city,
    COUNT(customer_id) AS sum_users
FROM 
    sakila.staff st
INNER JOIN 
    sakila.store sto ON sto.store_id = st.store_id
INNER JOIN 
    sakila.customer cus ON cus.store_id = sto.store_id
inner join
	sakila.address ad on ad.address_id  = st.address_id
INNER join
	sakila.city ci on ci.city_id = ad.city_id
GROUP BY 
    st.last_name,
    st.first_name,
    ci.city
HAVING 
    COUNT(customer_id) > 300

---


### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

### Решение 2
[length](https://github.com/sash3939/SQL-part2/assets/156709540/f6889261-3a17-490f-923a-5dda11860e6b)

SELECT COUNT(*) AS more_avg_length
FROM sakila.film
WHERE length > (SELECT AVG('length') FROM sakila.film);

---

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

### Решение 3
[rental](https://github.com/sash3939/SQL-part2/assets/156709540/5cd104de-4adf-4fe3-97a9-44e4b2a8336a)

SELECT 
    DATE_FORMAT(p.payment_date, '%Y-%m') AS month,
    SUM(p.amount) AS max_payment_amount,
    COUNT(r.rental_id) AS rental_count
FROM 
    sakila.payment p
JOIN 
    sakila.rental r ON DATE_FORMAT(p.payment_date, '%Y-%m') = DATE_FORMAT(r.rental_date, '%Y-%m')
GROUP BY 
    DATE_FORMAT(p.payment_date, '%Y-%m')
ORDER BY 
    max_payment_amount DESC
LIMIT 1;

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

[Премия](https://github.com/sash3939/SQL-part2/assets/156709540/98c6aa8c-5d47-4d6c-b9bb-ecaea2febfb1)

SELECT
    staff_id,
    COUNT(*) AS sales_count,
    CASE
        WHEN COUNT(*) > 8000 THEN 'Да'
        ELSE 'Нет'
    END AS Премия
FROM sakila.rental
GROUP BY staff_id;

### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.

### Решение 5*

[no_rental](https://github.com/sash3939/SQL-part2/assets/156709540/78c52013-9090-4425-9061-9bf604e148b2)


SELECT f.film_id, f.title
FROM sakila.film AS f
LEFT JOIN sakila.rental AS r ON r.inventory_id = f.film_id 
left JOIN sakila.inventory as i ON r.inventory_id = f.film_id
WHERE r.inventory_id  IS NULL;
