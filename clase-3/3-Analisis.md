```sql
-- A) OPERACIÓN DIARIA
-- 1) Socios más activos: Nombre, Apellido, Total de préstamos (DESC)

SELECT * FROM Socios;
SELECT SocioID, Nombre, Apellido FROM Socios;

SELECT * FROM Prestamos;

SELECT 
    Socios.SocioID, 
    Socios.Nombre, 
    Socios.Apellido, 
    Prestamos.PrestamoID
FROM Socios
JOIN Prestamos ON Prestamos.SocioID = Socios.SocioID;

SELECT 
    Socios.SocioID, 
    Socios.Nombre, 
    Socios.Apellido, 
    COUNT(Prestamos.PrestamoID) AS TotalPrestamos
FROM Socios
JOIN Prestamos ON Prestamos.SocioID = Socios.SocioID
GROUP BY Socios.SocioID, Socios.Nombre, Socios.Apellido
ORDER BY TotalPrestamos DESC; -- ASC (menor a mayor)

-- Socios sin préstamos: Nombre, Apellido, Email

SELECT * FROM Socios
LEFT JOIN Prestamos ON Prestamos.SocioID = Socios.SocioID;

SELECT 
    Socios.Nombre, 
    Socios.Apellido, 
    Socios.Email 
FROM Socios
LEFT JOIN Prestamos ON Prestamos.SocioID = Socios.SocioID
WHERE Prestamos.PrestamoID IS NULL;

-- Libros sin ejemplares

SELECT * FROM Libros;
SELECT * FROM Ejemplares;

SELECT * 
FROM Libros
LEFT JOIN Ejemplares ON Ejemplares.LibroID = Libros.LibroID
WHERE Ejemplares.EjemplarID IS NULL;

SELECT 
    Libros.Titulo 
FROM Libros
LEFT JOIN Ejemplares ON Ejemplares.LibroID = Libros.LibroID
WHERE Ejemplares.EjemplarID IS NULL;


```