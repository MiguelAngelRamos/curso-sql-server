```sql
CREATE OR ALTER FUNCTION Inventario.fn_ObtenerStock
(
    @ProductoID INT
)
RETURNS INT
AS
BEGIN
    DECLARE @Stock INT;
    
    SELECT @Stock = Stock
    FROM Inventario.Producto
    WHERE ProductoID = @ProductoID;
    
    RETURN ISNULL(@Stock, 0);
END
-- Llamar a la función 
-- SELECT Inventario.fn_ObtenerStock(1) AS StockActual;
```


## TABLA

```sql

CREATE OR ALTER FUNCTION Inventario.fn_ProductosPorCategoria
(
    @CategoriaID INT
)
RETURNS TABLE
AS
RETURN
(
    SELECT 
        ProductoID,
        Nombre,
        Precio,
        Stock
    FROM Inventario.Producto
    WHERE CategoriaID = @CategoriaID
);

-- Llamar a la función 
-- SELECT * FROM Inventario.fn_ProductosPorCategoria(1);
```