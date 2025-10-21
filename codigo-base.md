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