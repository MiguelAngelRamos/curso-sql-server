```sql
-- Creación de la tabla Instructores
CREATE TABLE Instructores (
    Id INT PRIMARY KEY IDENTITY(1,1),
    Nombre NVARCHAR(100) NOT NULL,
    Email NVARCHAR(100) NOT NULL UNIQUE
);

-- Creación de la tabla Cursos
CREATE TABLE Cursos (
    Id INT PRIMARY KEY IDENTITY(1,1),
    Titulo NVARCHAR(100) NOT NULL,
    Descripcion NVARCHAR(MAX),
    InstructorId INT,
    CONSTRAINT FK_Instructor FOREIGN KEY (InstructorId)
    REFERENCES Instructores(Id)
);

```

## Insertar informacion en las tablas:

```sql
-- Insertar instructores
INSERT INTO Instructores (Nombre, Email) VALUES
('Juan Pérez', 'juan.perez@example.com'),
('María López', 'maria.lopez@example.com');

-- Insertar cursos
INSERT INTO Cursos (Titulo, Descripcion, InstructorId) VALUES
('Curso de Python', 'Aprende Python desde cero', 1),
('Curso de PostgreSQL', 'Bases de datos con PostgreSQL', 2),
('Curso de JavaScript', 'Desarrollo web con JavaScript', 1);

```