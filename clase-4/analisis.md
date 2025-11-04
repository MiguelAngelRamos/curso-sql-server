```sql

-- Selecciona todos los usuarios
SELECT * FROM users;

-- Obtener todos los registros de la ultima conexion con la IP '221.X.X.X.'
SELECT first_name, last_name, last_connection FROM users where last_connection LIKE '221.%.%.%';

-- Obtener el first_name, last_name y la cantidad de followers(seguidores) entre mayor o igual 4600 y menor o igual 4700

SELECT first_name, last_name, followers 
FROM users 
WHERE followers BETWEEN 4600 AND 4700
ORDER BY
	followers asc;

-- Operadores de comparacion ><

SELECT first_name, last_name, followers 
FROM users 
WHERE followers >= 4600 AND followers <= 4700 -- 4600 AND 4700
ORDER BY followers asc;

-- Contar cuantos registros tiene la tabla users
SELECT COUNT(*) as total_users from users;

-- Obtener el numero total de usuarios(users) y el minimo de seguidores(followers)
SELECT COUNT(*) as total_users, MIN(followers) as min_followers from users;

-- Obtener el numero total de usuarios(users) y el maximo de seguidores(followers)
SELECT COUNT(*) as total_users, MAX(followers) as max_followers from users;

-- Obtener el nombre, apellido y numero de seguidores de los usuarios con menos followers (sub-consulta)

SELECT 
	first_name,
	last_name,
	followers
FROM users
WHERE
    followers = (SELECT min(followers) from users);


-- Obtener el nombre, apellido y numero de seguidores de los usuarios con maximo followers (sub-consulta)
SELECT 
	first_name,
	last_name,
	followers
FROM users
WHERE
    followers = (SELECT max(followers) from users);

-- Consulta HAVING <>
SELECT 
	COUNT(*) AS users, 
	country 
FROM users 
GROUP BY country 
HAVING COUNT(*) > 5
ORDER BY COUNT(*) desc;

-- BETWEEN 1 Y 5
SELECT 
	COUNT(*) AS users, 
	country 
FROM users 
GROUP BY country 
HAVING COUNT(*) BETWEEN 1 AND 5
ORDER BY COUNT(*) desc;

SELECT SUBSTRING('Hello, World!', 8,5);

-- Contar dominios dentro de la columna email @
SELECT 
COUNT(*) AS total,
SUBSTRING(email, CHARINDEX('@', email) + 1, LEN(email) - CHARINDEX('@', email)) AS domain
FROM users 
GROUP BY SUBSTRING(email, CHARINDEX('@', email) + 1, LEN(email) - CHARINDEX('@', email))
HAVING 
    COUNT(*) > 1
ORDER BY
  domain asc;

```