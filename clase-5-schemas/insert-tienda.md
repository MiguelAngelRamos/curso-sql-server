## Insertar Datos

```sql
-- Insertar categorías
INSERT INTO Inventario.Categoria (Nombre) VALUES
('Electrónica'),
('Ropa'),
('Alimentos');

PRINT 'Categorías insertadas';

-- Insertar productos
INSERT INTO Inventario.Producto (Nombre, CategoriaID, Precio, Stock) VALUES
('Laptop HP', 1, 800.00, 10),
('Mouse Logitech', 1, 25.00, 50),
('Teclado Mecánico', 1, 120.00, 30),
('Camisa Polo', 2, 35.00, 100),
('Pantalón Jeans', 2, 45.00, 80),
('Chocolate', 3, 2.50, 200),
('Café Premium', 3, 8.00, 150);

PRINT ' Productos insertados';

```