```sql
CREATE PROCEDURE Ventas.sp_VenderProducto
    @ProductoID INT,
    @Cantidad INT,
    @VentaID INT OUTPUT
AS
BEGIN
    SET NOCOUNT ON;
    
    DECLARE @StockActual INT;
    DECLARE @Precio DECIMAL(10,2);
    DECLARE @Total DECIMAL(10,2);
    
    BEGIN TRY
        BEGIN TRANSACTION;
        
        -- Obtener stock y precio con bloqueo
        SELECT 
            @StockActual = Stock,
            @Precio = Precio
        FROM Inventario.Producto WITH (UPDLOCK)
        WHERE ProductoID = @ProductoID;
        
        -- Validar que el producto existe
        IF @StockActual IS NULL
        BEGIN
            RAISERROR('Producto no encontrado', 16, 1);
            RETURN;
        END
        
        -- Validar cantidad
        IF @Cantidad <= 0
        BEGIN
            RAISERROR('La cantidad debe ser mayor a cero', 16, 1);
            RETURN;
        END
        
        -- Validar stock suficiente
        IF @StockActual < @Cantidad
        BEGIN
            RAISERROR('Stock insuficiente', 16, 1);
            RETURN;
        END
        
        -- Calcular total
        SET @Total = @Precio * @Cantidad;
        
        -- Reducir stock
        UPDATE Inventario.Producto
        SET Stock = Stock - @Cantidad
        WHERE ProductoID = @ProductoID;
        
        -- Crear venta
        INSERT INTO Ventas.Venta (Total)
        VALUES (@Total);
        
        SET @VentaID = SCOPE_IDENTITY();
        
        -- Insertar detalle
        INSERT INTO Ventas.DetalleVenta (VentaID, ProductoID, Cantidad, PrecioUnitario)
        VALUES (@VentaID, @ProductoID, @Cantidad, @Precio);
        
        COMMIT TRANSACTION;
        
        PRINT 'Venta realizada exitosamente';
        PRINT 'ID de Venta: ' + CAST(@VentaID AS NVARCHAR);
        PRINT 'Total: $' + CAST(@Total AS NVARCHAR);
    END TRY
    BEGIN CATCH
        IF @@TRANCOUNT > 0
            ROLLBACK TRANSACTION;
        
        DECLARE @ErrorMsg NVARCHAR(4000) = ERROR_MESSAGE();
        RAISERROR(@ErrorMsg, 16, 1);
    END CATCH
END
GO

```