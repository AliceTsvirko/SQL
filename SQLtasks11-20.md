Задание: 11 
Найдите среднюю скорость ПК.

SELECT AVG(speed) AS avg_speed FROM PC

Задание: 12 
Найдите среднюю скорость ПК-блокнотов, цена которых превышает 1000 дол.

SELECT AVG(speed) as avg_speed FROM Laptop
WHERE Laptop.price > ‘1000’

Задание: 13 Найдите среднюю скорость ПК, выпущенных производителем A.

SELECT AVG(speed) AS avg_speed
FROM PC
INNER JOIN
Product ON PC.model = Product.model AND Product.maker = 'A'

Задание: 14 
Найдите класс, имя и страну для кораблей из таблицы Ships, имеющих не менее 10 орудий. 

SELECT Ships.class, name, country
FROM SHIPS
INNER JOIN
Classes ON Ships.class = Classes.class AND Classes.numguns >= '10'

Задание: 15 Найдите размеры жестких дисков, совпадающих у двух и более PC. Вывести: HD 

SELECT hd FROM PC
GROUP BY PC.hd
HAVING COUNT(HD) > ‘1’

Задание: 16 
Найдите пары моделей PC, имеющих одинаковые скорость и RAM. В результате каждая пара указывается только один раз, т.е. (i,j), но не (j,i), Порядок вывода: модель с большим номером, модель с меньшим номером, скорость и RAM. 


Задание: 17 
Найдите модели ПК-блокнотов, скорость которых меньше скорости каждого из ПК. 
Вывести: type, model, speed 
SELECT DISTINCT Product.type, Laptop.model, Laptop.speed 
FROM Product, Laptop
WHERE Product.type = 'laptop' AND Laptop.speed < ALL (SELECT speed 
 FROM PC)
Задание: 18 
Найдите производителей самых дешевых цветных принтеров. Вывести: maker, price.

SELECT MAX(model) AS 'model', MIN(model) AS 'model', speed, ram
FROM PC
GROUP BY speed, ram
HAVING MAX(model) > MIN(model);

Задание: 19 
Для каждого производителя, имеющего модели в таблице Laptop, найдите средний размер экрана выпускаемых им ПК-блокнотов. 
Вывести: maker, средний размер экрана. 

SELECT maker, AVG(screen) AS avg_screen
FROM Product
INNER JOIN
Laptop ON Product.model = Laptop.model AND Product.type = 'laptop'
GROUP BY MAKER

Задание: 20 
Найдите производителей, выпускающих по меньшей мере три различных модели ПК. Вывести: Maker, число моделей ПК. 

SELECT maker, COUNT(model) AS count_model 
FROM Product
WHERE type = 'PC' 
GROUP BY maker
HAVING COUNT(model) >= '3'

