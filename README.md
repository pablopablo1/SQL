# SQL

Базовый синтаксис SQL запроса

Выведите поле name из таблицы Passenger.

SELECT name FROM passenger 

Выведите поля member_id, member_name и status из таблицы FamilyMembers.

SELECT member_id , member_name , status FROM FamilyMembers

Выведите все столбцы из таблицы Payments.

SELECT * FROM Payments

Выполните предложенный ниже запрос, чтобы увидеть список пассажиров. Если присмотреться, то можно увидеть, что пассажир с именем Bruce Willis присутствует здесь дважды (в начале и конце списка).

SELECT DISTINCT name FROM Passenger


====


Условный оператор WHERE

Выведите идентификаторы товаров (поле good) из таблицы Payments, стоимость которых больше 2000 единиц. Стоимость товара хранится в поле unit_price.

SELECT good  FROM Payments
WHERE unit_price > 2000

Выведите имена (поле member_name) членов семьи из таблицы FamilyMembers, чей статус (поле status) равен "father".

SELECT member_name FROM FamilyMembers
WHERE status = 'father'

Выведите имя (поле member_name) и дату рождения (поле birthday) членов семьи из таблицы FamilyMembers, чей статус (поле status) равен "father" или "mother".

SELECT member_name,birthday FROM FamilyMembers
WHERE status IN ('father','mother') 

Необходимо получить все комнаты, в которых есть как кухня (поле has_kitchen), так и интернет (поле has_internet). Напишите запрос, удовлетворяющий вышеописанному условию, который выводит все поля из таблицы Rooms.

SELECT * FROM Rooms
WHERE has_kitchen * has_internet =1

Выведите резервации комнат (поля room_id, start_date, end_date) из таблицы Reservations, у которых итоговая стоимость аренды (поле total) находится в промежутке от 500 до 1200 включительно.

SELECT room_id, start_date, end_date FROM Reservations
WHERE total BETWEEN 500 AND 1200

Выведите всех членов семьи с фамилией "Quincey".

SELECT member_name 
FROM FamilyMembers
WHERE member_name LIKE '% Quincey'


====


Сортировка, оператор ORDER BY

Для каждого отдельного платежа выведите идентификатор товара и сумму, потраченную на него, в отсортированном по убыванию этой суммы виде. Список платежей находится в таблице Payments.
Для вывода суммы используйте псевдоним sum.

SELECT 
         p.good, 
         p.unit_price*p.amount AS 'sum'
FROM Payments p
ORDER BY p.unit_price*p.amount DESC

Выведите список членов семьи с фамилией Quincey, в отсортированном по возрастанию столбцам status и member_name виде.

SELECT * FROM FamilyMembers
WHERE member_name LIKE ('% Quincey' )
ORDER BY status ASC, member_name ASC


====

Группировка данных и агрегатные функции

Подсчитайте количество учеников в каждом классе, а также отсортируйте их по убыванию количества учеников. Принадлежность ученика к конкретному классу вы можете получить из таблицы Student_in_class. В качестве результата необходимо вывести идентификатор класса (поле class) и количество учеников в этом классе.

SELECT class, COUNT( * ) AS count  FROM Student_in_class 
GROUP BY class 
ORDER BY count DESC

Найдите самых старших членов семьи (используйте поле birthday) среди всех существующих семей на основании их статуса (поле status). Выведите статус и дату рождения.

SELECT status, MIN (birthday) AS birthday  FROM FamilyMembers
GROUP BY status 

Получите среднее время полётов, совершённых на каждой из моделей самолёта. Выведите поле plane и среднее время полёта в секундах.

SELECT plane, AVG ( TIMESTAMPDIFF ( second, time_out, time_in ) ) AS time FROM trip
GROUP BY plane 

Выведите идентификатор комнаты (поле room_id), среднюю стоимость за один день аренды (поле price, для вывода используйте псевдоним avg_price), а также количество резерваций этой комнаты (используйте псевдоним count).

SELECT room_id, AVG ( price ) AS avg_price, COUNT ( room_id ) AS count FROM Reservations
GROUP BY room_id 
ORDER BY count DESC, avg_price DESC

Дополните запрос из предыдущего задания, оставив в выборке только те комнаты, чья средняя стоимость аренды превышает 150 ед.

SELECT room_id, AVG ( price ) AS avg_price, COUNT ( room_id ) AS count 
FROM Reservations
GROUP BY room_id 
HAVING avg_price > '150'
ORDER BY count DESC, avg_price DESC


====


Многотабличные запросы, оператор JOIN

Объедините таблицы Class и Student_in_class с помощью внутреннего соединения по полям Class.id и Student_in_class.class. Выведите название класса (поле Class.name) и идентификатор ученика (поле Student_in_class.student).

SELECT Class.name,
       Student_in_class.student
FROM CLASS
JOIN Student_in_class ON class.id=Student_in_class.class

Дополните запрос из предыдущего задания, добавив ещё одно внутреннее соединение с таблицей Student. Объедините по полям Student_in_class.student и Student.id и вместо идентификатора ученика выведите его имя (поле first_name).

SELECT Class.name,
       student.first_name
FROM CLASS
JOIN Student_in_class 
       ON class.id=Student_in_class.class
JOIN Student 
       ON Student_in_class.student = Student.id

Выведите названия продуктов, которые покупал член семьи со статусом "son". Для получения выборки вам нужно объединить таблицу Payments с таблицей FamilyMembers по полям family_member и member_id, а также с таблицей Goods по полям good и good_id.

SELECT good_name
FROM Goods
JOIN Payments ON good = good_id 
JOIN FamilyMembers ON family_member = member_id 
WHERE status = 'son'

Выведите идентификатор (поле room_id) и среднюю оценку комнаты (поле rating, для вывода используйте псевдоним avg_score), составленную на основании отзывов из таблицы Reviews.

SELECT room_id, AVG ( rating ) AS avg_score
FROM Reviews
JOIN Reservations ON reservation_id = Reservations.id 
GROUP BY room_id 


====


Ограничение выборки, оператор LIMIT

Отсортируйте список компаний (таблица Company) по их названию в алфавитном порядке и выведите первые две записи.







====


Вложенные SQL запросы

Выведите список комнат (все поля, таблица Rooms), которые по своим удобствам (has_tv, has_internet, has_kitchen, has_air_con) совпадают с комнатой с идентификатором "11".

SELECT * FROM Rooms
WHERE ( has_tv, has_internet, has_kitchen, has_air_con )=
(
SELECT has_tv , has_internet , has_kitchen , has_air_con
FROM Rooms
WHERE ID =11
)


Выведите названия товаров из таблицы Goods (поле good_name), которые ещё ни разу не покупались ни одним из членов семьи (таблица Payments).

SELECT good_name FROM Goods
WHERE Goods.good_id   NOT IN (SELECT good  FROM Payments )
