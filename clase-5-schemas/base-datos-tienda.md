
## CREANDO DE LA BASE DE DATOS

```sql

CREATE DATABASE Tienda;

USE Tienda;

```
## Creando los Schemas

```sql
-- CREAR ESQUEMAS
CREATE SCHEMA Inventario AUTHORIZATION dbo;
CREATE SCHEMA Ventas AUTHORIZATION dbo;

```

## Creación de las tablas

```sql
CREATE TABLE Inventario.Categoria (
    CategoriaID INT IDENTITY(1,1) PRIMARY KEY,
    Nombre NVARCHAR(50) NOT NULL UNIQUE
);
GO

PRINT 'Tabla Inventario.Categoria creada';

CREATE TABLE Inventario.Producto (
    ProductoID INT IDENTITY(1,1) PRIMARY KEY,
    Nombre NVARCHAR(100) NOT NULL,
    CategoriaID INT NOT NULL,
    Precio DECIMAL(10,2) NOT NULL CHECK (Precio > 0),
    Stock INT NOT NULL DEFAULT 0 CHECK (Stock >= 0),
    CONSTRAINT FK_Producto_Categoria FOREIGN KEY (CategoriaID) 
        REFERENCES Inventario.Categoria(CategoriaID)
);

GO
PRINT 'Tabla Inventario.Producto creada';

CREATE TABLE Ventas.Venta (
    VentaID INT IDENTITY(1,1) PRIMARY KEY,
    Fecha DATETIME2 NOT NULL DEFAULT GETDATE(),
    Total DECIMAL(10,2) NOT NULL DEFAULT 0
);
GO

PRINT 'Tabla Ventas.Venta creada';

-- Tabla de detalle de ventas (qué productos se vendieron)
CREATE TABLE Ventas.DetalleVenta (
    DetalleID INT IDENTITY(1,1) PRIMARY KEY,
    VentaID INT NOT NULL,
    ProductoID INT NOT NULL,
    Cantidad INT NOT NULL CHECK (Cantidad > 0),
    PrecioUnitario DECIMAL(10,2) NOT NULL,
    Subtotal AS (Cantidad * PrecioUnitario) PERSISTED,
    CONSTRAINT FK_Detalle_Venta FOREIGN KEY (VentaID) 
        REFERENCES Ventas.Venta(VentaID),
    CONSTRAINT FK_Detalle_Producto FOREIGN KEY (ProductoID) 
        REFERENCES Inventario.Producto(ProductoID)
);
GO

PRINT ' Tabla Ventas.DetalleVenta creada';
```