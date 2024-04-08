Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.


### Решение 1
[more_300](https://github.com/sash3939/SQL-part2/assets/156709540/1eaea02f-619b-4f6f-85a4-d88381366a48)

SELECT 
    e.last_name, 
    e.first_name, 
    s.address_id AS city,
    COUNT(customer_id) AS sum_users
FROM 
    sakila.staff e
JOIN 
    sakila.store s ON e.store_id = s.store_id
JOIN 
    sakila.customer c ON s.store_id = c.store_id
GROUP BY 
    e.last_name, 
    e.first_name, 
    s.address_id
HAVING 
    COUNT(c.customer_id) > 300;

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

### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.
