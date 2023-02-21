# SQL
Схема БД состоит из четырех таблиц:
Product(maker, model, type)
PC(code, model, speed, ram, hd, cd, price)
Laptop(code, model, speed, ram, hd, price, screen)
Printer(code, model, color, type, price)
Таблица Product представляет производителя (maker), номер модели (model) и тип ('PC' - ПК, 'Laptop' - ПК-блокнот или 'Printer' - принтер). Предполагается, что номера моделей в таблице Product уникальны для всех производителей и типов продуктов. В таблице PC для каждого ПК, однозначно определяемого уникальным кодом – code, указаны модель – model (внешний ключ к таблице Product), скорость - speed (процессора в мегагерцах), объем памяти - ram (в мегабайтах), размер диска - hd (в гигабайтах), скорость считывающего устройства - cd (например, '4x') и цена - price (в долларах). Таблица Laptop аналогична таблице РС за исключением того, что вместо скорости CD содержит размер экрана -screen (в дюймах). В таблице Printer для каждой модели принтера указывается, является ли он цветным - color ('y', если цветной), тип принтера - type (лазерный – 'Laser', струйный – 'Jet' или матричный – 'Matrix') и цена - price.

Рассматривается БД кораблей, участвовавших во второй мировой войне. Имеются следующие отношения:
Classes (class, type, country, numGuns, bore, displacement)
Ships (name, class, launched)
Battles (name, date)
Outcomes (ship, battle, result) 
Корабли в «классах» построены по одному и тому же проекту, и классу присваивается либо имя первого корабля, построенного по данному проекту, либо названию класса дается имя проекта, которое не совпадает ни с одним из кораблей в БД. Корабль, давший название классу, называется головным.
Отношение Classes содержит имя класса, тип (bb для боевого (линейного) корабля или bc для боевого крейсера), страну, в которой построен корабль, число главных орудий, калибр орудий (диаметр ствола орудия в дюймах) и водоизмещение ( вес в тоннах). В отношении Ships записаны название корабля, имя его класса и год спуска на воду. В отношение Battles включены название и дата битвы, в которой участвовали корабли, а в отношении Outcomes – результат участия данного корабля в битве (потоплен-sunk, поврежден - damaged или невредим - OK). 
Замечания. 1) В отношение Outcomes могут входить корабли, отсутствующие в отношении Ships. 2) Потопленный корабль в последующих битвах участия не принимает.
Задание: 1 Найдите номер модели, скорость и размер жесткого диска для всех ПК стоимостью менее 500 дол. Вывести: model, speed и hd.

SELECT MODEL, SPEED, HD FROM PC
WHERE PRICE < '500'

Задание: 2 Найдите производителей принтеров. Вывести: maker.

SELECT DISTINCT maker FROM Product
WHERE type='Printer'

Задание: 3 Найдите номер модели, объем памяти и размеры экранов ПК-блокнотов, цена которых превышает 1000 дол. 

SELECT MODEL, RAM, SCREEN FROM LAPTOP
WHERE PRICE > '1000'

Задание: 4 
Найдите все записи таблицы Printer для цветных принтеров. 

SELECT * FROM PRINTER
WHERE COLOR = 'Y'



Задание: 5 
Найдите номер модели, скорость и размер жесткого диска ПК, имеющих 12x или 24x CD и цену менее 600 дол. 

SELECT MODEL, SPEED, HD FROM PC
WHERE (CD = '12X' OR CD = '24X') AND PRICE < 600

Задание: 6 
Для каждого производителя, выпускающего ПК-блокноты c объёмом жесткого диска не менее 10 Гбайт, найти скорости таких ПК-блокнотов. Вывод: производитель, скорость.

SELECT DISTINCT maker, speed
FROM Product JOIN 
LAPTOP ON LAPTOP.model = Product.modeL AND LAPTOP.hd >= '10'

Задание: 7 
Найдите номера моделей и цены всех имеющихся в продаже продуктов (любого типа) производителя B (латинская буква). 

1.	SELECT model, price 
2.	FROM PC 
3.	WHERE model = (SELECT model 
4.	 FROM Product 
5.	 WHERE maker = 'B' AND 
6.	 type = 'PC'
7.	 )
8.	UNION
9.	SELECT model, price 
10.	FROM Laptop 
11.	WHERE model = (SELECT model 
12.	 FROM Product 
13.	 WHERE maker = 'B' AND 
14.	 type = 'Laptop'
15.	 )
16.	UNION
17.	SELECT model, price 
18.	FROM Printer 
19.	WHERE model = (SELECT model 
20.	 FROM Product 
21.	 WHERE maker = 'B' AND 
22.	 type = 'Printer'
23.	 );

Задание: 8 
Найдите производителя, выпускающего ПК, но не ПК-блокноты.

SELECT MAKER FROM PRODUCT
WHERE TYPE = 'PC'
EXCEPT SELECT MAKER FROM PRODUCT
WHERE TYPE = 'LAPTOP'

Задание: 9 
Найдите производителей ПК с процессором не менее 450 Мгц. Вывести: Maker

SELECT DISTINCT maker
FROM Product 
INNER JOIN 
PC ON PC.model = Product.model AND PC.SPEED >= 450

Задание: 10 
Найдите модели принтеров, имеющих самую высокую цену. Вывести: model, price 

SELECT MODEL, PRICE FROM PRINTER
WHERE PRICE = (SELECT MAX(PRICE) FROM PRINTER)

Задание: 11 
Найдите среднюю скорость ПК.

SELECT AVG(SPEED) AS AVG_SPEED FROM PC

Задание: 12 
Найдите среднюю скорость ПК-блокнотов, цена которых превышает 1000 дол.

SELECT AVG(SPEED) as Avg_price FROM LAPTOP
WHERE laptop.price > 1000

Задание: 13 Найдите среднюю скорость ПК, выпущенных производителем A.

SELECT AVG(SPEED) AS AVG_SPEED
FROM PC
INNER JOIN
PRODUCT ON PC.MODEL = PRODUCT.MODEL AND PRODUCT.MAKER = 'A'

Задание: 14 
Найдите класс, имя и страну для кораблей из таблицы Ships, имеющих не менее 10 орудий. 

SELECT SHIPS.CLASS, NAME, COUNTRY
FROM SHIPS
INNER JOIN
CLASSES ON SHIPS.CLASS = CLASSES.CLASS AND CLASSES.NUMGUNS >= '10'

Задание: 15 Найдите размеры жестких дисков, совпадающих у двух и более PC. Вывести: HD 

SELECT HD FROM PC
GROUP BY PC.HD
HAVING COUNT(HD) > 1

Задание: 16 
Найдите пары моделей PC, имеющих одинаковые скорость и RAM. В результате каждая пара указывается только один раз, т.е. (i,j), но не (j,i), Порядок вывода: модель с большим номером, модель с меньшим номером, скорость и RAM. 


Задание: 17 
Найдите модели ПК-блокнотов, скорость которых меньше скорости каждого из ПК. 
Вывести: type, model, speed 
SELECT DISTINCT PRODUCT.TYPE, LAPTOP.MODEL, LAPTOP.SPEED FROM PRODUCT, LAPTOP
WHERE PRODUCT.TYPE = 'LAPTOP' AND LAPTOP.SPEED < ALL (SELECT SPEED 
 FROM PC)
Задание: 18 
Найдите производителей самых дешевых цветных принтеров. Вывести: maker, price.

1.	SELECT MAX(model) AS 'model', MIN(model) AS 'model', speed, ram
2.	FROM PC
3.	GROUP BY speed, ram
4.	HAVING MAX(model) > MIN(model);

Задание: 19 
Для каждого производителя, имеющего модели в таблице Laptop, найдите средний размер экрана выпускаемых им ПК-блокнотов. 
Вывести: maker, средний размер экрана. 

SELECT MAKER, AVG(SCREEN) AS AVG_SCREEN
FROM PRODUCT
INNER JOIN
LAPTOP ON PRODUCT.MODEL = LAPTOP.MODEL AND PRODUCT.TYPE = 'LAPTOP'
GROUP BY MAKER

Задание: 20 
Найдите производителей, выпускающих по меньшей мере три различных модели ПК. Вывести: Maker, число моделей ПК. 

SELECT MAKER, COUNT(MODEL) AS COUNT_MODEL FROM PRODUCT
WHERE TYPE = 'PC' 
GROUP BY MAKER
HAVING COUNT(MODEL) >= '3'

Задание: 21
Найдите максимальную цену ПК, выпускаемых каждым производителем, у которого есть модели в таблице PC. 
Вывести: maker, максимальная цена. 

SELECT MAKER, MAX(PRICE) AS MAX_PRICE
FROM PRODUCT
INNER JOIN 
PC ON PRODUCT.MODEL = PC.MODEL AND PRODUCT.TYPE = 'PC'
GROUP BY MAKER

Задание: 22 
Для каждого значения скорости ПК, превышающего 600 МГц, определите среднюю цену ПК с такой же скоростью. Вывести: speed, средняя цена. 

SELECT SPEED, AVG(PRICE) AS AVG_PRICE 
FROM PC
WHERE SPEED > '600'
GROUP BY SPEED

Задание: 23 
Найдите производителей, которые производили бы как ПК
со скоростью не менее 750 МГц, так и ПК-блокноты со скоростью не менее 750 МГц.
Вывести: Maker

SELECT maker FROM (
    SELECT maker FROM Product INNER JOIN PC ON PRODUCT.MODEL = PC.MODEL WHERE SPEED>='750'
    INTERSECT
    SELECT maker FROM Product INNER JOIN LAPTOP ON PRODUCT.MODEL = LAPTOP.MODEL WHERE SPEED >='750'
    ) X GROUP BY maker

Задание: 24 Перечислите номера моделей любых типов, имеющих самую высокую цену по всей имеющейся в базе данных продукции. 

Задание: 25 
Найдите производителей принтеров, которые производят ПК с наименьшим объемом RAM и с самым быстрым процессором среди всех ПК, имеющих наименьший объем RAM. Вывести: Maker 

Задание: 26 
Найдите среднюю цену ПК и ПК-блокнотов, выпущенных производителем A (латинская буква). Вывести: одна общая средняя цена.

Задание: 27 
Найдите средний размер диска ПК каждого из тех производителей, которые выпускают и принтеры. Вывести: maker, средний размер HD.

Задание: 28 Используя таблицу Product, определить количество производителей, выпускающих по одной модели.

Задание: 29 
В предположении, что приход и расход денег на каждом пункте приема фиксируется не чаще одного раза в день [т.е. первичный ключ (пункт, дата)], написать запрос с выходными данными (пункт, дата, приход, расход). Использовать таблицы Income_o и Outcome_o. 

Задание: 30 В предположении, что приход и расход денег на каждом пункте приема фиксируется произвольное число раз (первичным ключом в таблицах является столбец code), требуется получить таблицу, в которой каждому пункту за каждую дату выполнения операций будет соответствовать одна строка.
Вывод: point, date, суммарный расход пункта за день (out), суммарный приход пункта за день (inc). Отсутствующие значения считать неопределенными (NULL). 

Задание: 31 
Для классов кораблей, калибр орудий которых не менее 16 дюймов, укажите класс и страну.

SELECT CLASS, COUNTRY
FROM CLASSES
WHERE BORE >= '16'

Задание: 32
Одной из характеристик корабля является половина куба калибра его главных орудий (mw). С точностью до 2 десятичных знаков определите среднее значение mw для кораблей каждой страны, у которой есть корабли в базе данных.

Задание: 33 
Укажите корабли, потопленные в сражениях в Северной Атлантике (North Atlantic). Вывод: ship. 

SELECT SHIP
FROM OUTCOMES
WHERE RESULT = 'SUNK' AND BATTLE = 'NORTH ATLANTIC'

Задание: 34 По Вашингтонскому международному договору от начала 1922 г. запрещалось строить линейные корабли водоизмещением более 35 тыс.тонн. Укажите корабли, нарушившие этот договор (учитывать только корабли c известным годом спуска на воду). Вывести названия кораблей.

Задание: 35 
В таблице Product найти модели, которые состоят только из цифр или только из латинских букв (A-Z, без учета регистра).
Вывод: номер модели, тип модели. 

SELECT model, type FROM Product 
WHERE model NOT LIKE '%[^A-Z]%' 
OR model NOT LIKE '%[^0-9]%'

Задание: 36 
Перечислите названия головных кораблей, имеющихся в базе данных (учесть корабли в Outcomes). 

Задание: 37 
Найдите классы, в которые входит только один корабль из базы данных (учесть также корабли в Outcomes).

Задание: 38 
Найдите страны, имевшие когда-либо классы обычных боевых кораблей ('bb') и имевшие когда-либо классы крейсеров ('bc').

Задание: 39 

Найдите корабли, `сохранившиеся для будущих сражений`; т.е. выведенные из строя в одной битве (damaged), они участвовали в другой, произошедшей позже.
Задание: 40 
Найти производителей, которые выпускают более одной модели, при этом все выпускаемые производителем модели являются продуктами одного типа.
Вывести: maker, type 






