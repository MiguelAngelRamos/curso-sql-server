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
```