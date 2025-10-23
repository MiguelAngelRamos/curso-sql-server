
## Creando la base de datos
```sql
CREATE DATABASE AnaliticaSimple;
```

## Para utilizar la base de datos

```sql
USE AnaliticaSimple;
```

```sql
CREATE TABLE Customers (
    CustomerID INT IDENTITY(1,1) PRIMARY KEY,
    FullName NVARCHAR(100) NOT NULL,
    Email NVARCHAR(100) NULL
);

CREATE TABLE Products (
    ProductID INT IDENTITY(1,1) PRIMARY KEY,
    ProductName NVARCHAR(100) NOT NULL,
    Price DECIMAL(10,2) NOT NULL
);

CREATE TABLE Orders (
    OrderID INT IDENTITY(1,1) PRIMARY KEY,
    CustomerID INT NULL,
    OrderDate DATE NOT NULL DEFAULT (GETDATE()),
    CONSTRAINT FK_Orders_Customers FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);

CREATE TABLE OrderItems (
    OrderItemID INT IDENTITY(1,1) PRIMARY KEY,
    OrderID INT NOT NULL,
    ProductID INT NOT NULL,
    Quantity INT NOT NULL CHECK (Quantity > 0),
    UnitPrice DECIMAL(10,2) NOT NULL,
    LineTotal AS (Quantity * UnitPrice) PERSISTED,
    CONSTRAINT FK_OrderItems_Orders FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),
    CONSTRAINT FK_OrderItems_Products FOREIGN KEY (ProductID) REFERENCES Products(ProductID)
);
GO

```


## Insertar informacion en las tablas

```sql
-- Datos de ejemplo
-- Customers (8) - incluye algunos sin órdenes para LEFT JOIN demo
INSERT INTO Customers (FullName, Email) VALUES
(N'Carlos Gómez', 'carlos.gomez@example.com'),
(N'Marta Ruiz', 'marta.ruiz@example.com'),
(N'Laura Martín', 'laura.martin@example.com'),
(N'Pedro Lopez', 'pedro.lopez@example.com'),
(N'Isabel Navarro', 'isabel.navarro@example.com'),
(N'Fernando Ortega', 'fernando.ortega@example.com'),
(N'Sofía Moreira', 'sofia.moreira@example.com'),
(N'ClienteSinOrden', NULL);

-- Products (6) - incluye al menos un producto sin ventas
INSERT INTO Products (ProductName, Price) VALUES
(N'Smartphone X1', 699.00),
(N'Auriculares Bluetooth', 89.90),
(N'Cargador USB-C', 19.50),
(N'Aspiradora V5', 129.99),
(N'Escritorio M1', 199.00),
(N'ProductoSinVenta', 49.99);

-- Orders (6) - algunas órdenes sin CustomerID (posible dato faltante)
INSERT INTO Orders (CustomerID, OrderDate) VALUES
(1, '2025-01-05'),
(2, '2025-01-07'),
(3, '2025-02-10'),
(4, '2025-02-11'),
(NULL, '2025-03-01'),
(5, '2025-03-05');

-- OrderItems (12)
INSERT INTO OrderItems (OrderID, ProductID, Quantity, UnitPrice) VALUES
(1, 1, 1, 699.00),
(1, 3, 2, 19.50),
(2, 2, 1, 89.90),
(2, 6, 1, 49.99),
(3, 4, 1, 129.99),
(3, 3, 3, 19.50),
(4, 5, 1, 199.00),
(4, 2, 2, 89.90),
(5, 1, 1, 699.00),
(6, 3, 2, 19.50),
(6, 2, 1, 89.90),
(2, 3, 1, 19.50);
GO

```