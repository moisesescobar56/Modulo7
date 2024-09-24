# Diseño y programación de Ventas

## PARTE 1 - Diagrama FISICO y UML

![image](https://github.com/user-attachments/assets/3d0e06bf-58f0-4631-aede-f34009246078)

![image](https://github.com/user-attachments/assets/0af70dc5-c94c-4c59-aa77-f51dc12477fb)

Para este proceso del sistema se deben programar el Backend de los siguientes procesos. 
- **Empleado:**
  - Clases
  - Método Buscar() y ObtenerDiccionario()
- **Cliente:**
  - Clases
  - Método Buscar() y ObtenerDiccionario()
- **Producto:**
  - Clases
  - Método Buscar() y ObtenerDiccionario()
- **DetalleVenta:**
  - SP: SP_InsertarDetalleVenta
  - Clases
  - Métodos Guardar() y Buscar()
- **Venta:**
  - SP: SP_InsertarVenta, SP_ModificarVenta, SP_ObtenerVentaPorId
  - Clases
  - Métodos Guardar(), Modificar(), ObtenerPorId() y Buscar()

En el proceso de Guardar Venta se implementaran [**SqlTransaction**](https://learn.microsoft.com/en-us/dotnet/api/system.data.sqlclient.sqltransaction?view=netframework-4.8.1).

## PARTE 2 - Codificacion de Procedimientos almacenados

### TABLA VENTA

**Insertar Venta:**
```sql
-- Insertar Venta
CREATE PROCEDURE SP_InsertarVenta
	@IdEmpleado SMALLINT,
	@IdCliente INT,
    @Total DECIMAL(9,2)
AS
BEGIN
	-- Generar nuevo Correlativo 
	DECLARE @Correlativo BIGINT
	SET @Correlativo = (SELECT MAX(Correlativo) FROM Venta) + 1
	-- Verificar Correlativo generado
	IF EXISTS (SELECT * FROM Venta WHERE Correlativo = @Correlativo)
		BEGIN 
			PRINT 'Venta ya registrada, correlativo ya ocupado'
		END
	ELSE
		BEGIN
			-- ESTATUS: 1 = FACTURADO, 2 = ANULADO
			INSERT INTO Venta (IdEmpleado, IdCliente, Correlativo, FechaHora, Total, Estatus)
			VALUES (@IdEmpleado, @IdCliente, @Correlativo, GETDATE(), @Total, 1) 
			SELECT SCOPE_IDENTITY() -- Retorna el IdVenta (PK) registrado
		END
END
GO
-- Test de Insertar Venta
EXEC SP_InsertarVenta @IdEmpleado = 1, @IdCliente = 1, @Total = 2.25
GO
```

![image](https://github.com/user-attachments/assets/e4743759-0ee0-4ad9-a833-a8cb128f871c)

**Modificar Venta:**
```sql
-- Modificar Venta
CREATE PROCEDURE SP_ModificarVenta
    @IdVenta BIGINT,
    @Estatus TINYINT
AS
BEGIN
	IF EXISTS (SELECT * FROM Venta WHERE IdVenta = @IdVenta AND Estatus = @Estatus)
		BEGIN 
			PRINT 'Venta ya actualizada, ya tiene el mismo estatus'
		END
	ELSE
		BEGIN
			-- ESTATUS: 1 = FACTURADO, 2 = ANULADO
			UPDATE Venta
			SET Estatus = @Estatus
			WHERE IdVenta = @IdVenta;
			PRINT 'Venta modificada correctamente';
		END
END;
GO
-- Test de Modificar Venta
EXEC SP_ModificarVenta @IdVenta=1, @Estatus = 1
GO
```

![image](https://github.com/user-attachments/assets/c527f7cc-b9a4-441a-8dcf-e538b91cb2b5)

**Obtener Venta Por Id:**
```sql
-- Obtener Venta por ID
CREATE PROCEDURE SP_ObtenerVentaPorId
	@IdVenta BIGINT
AS
BEGIN
    SELECT TOP 1 IdVenta, IdEmpleado, IdCliente, Correlativo, FechaHora, Total, Estatus
    FROM Venta
	WHERE IdVenta = @IdVenta;
END;
GO
-- TestO btener Venta por ID
EXEC SP_ObtenerVentaPorId @IdVenta = 1
GO
```

![image](https://github.com/user-attachments/assets/0a0a3a55-b90d-4f04-a496-24dc806e7b01)

### TABLA DETALLE-VENTA

**Insertar DetalleVenta:**
```sql
-- Insertar DetalleVenta
CREATE PROCEDURE SP_InsertarDetalleVenta
	@IdVenta INT,
	@IdProducto INT,
    @Cantidad SMALLINT,
	@Precio DECIMAL (5,2),
    @Subtotal DECIMAL(9,2)
AS
BEGIN
	IF EXISTS (SELECT * FROM DetalleVenta WHERE IdVenta = @IdVenta AND IdProducto = @IdProducto)
		BEGIN 
			PRINT 'Producto ya agregado, detalle repetido'
		END
	ELSE
		BEGIN
			-- ESTATUS: 1 = FACTURADO, 2 = ANULADO
			INSERT INTO DetalleVenta (IdVenta, IdProducto, Cantidad, Precio, SubTotal)
			VALUES (@IdVenta, @IdProducto, @Cantidad, @Precio, @Subtotal) 
		END
END
GO
-- Test de Insertar DetalleVenta
EXEC SP_InsertarDetalleVenta @IdVenta = 3, @IdProducto = 8, @Cantidad = 3, @Precio = 0.75 , @Subtotal = 2.25
GO
```

![image](https://github.com/user-attachments/assets/63eee9c6-cb5c-487c-a8e3-4bfb2c1daaa2)

## PARTE 3 - Programación Entidades

**Categoria**

![image](https://github.com/user-attachments/assets/1b85fe0a-48ed-4713-b359-060f5d1f87a0)

**Producto**

![image](https://github.com/user-attachments/assets/cfc52597-5e75-4cfd-9783-2f4947216730)

**Cliente**

![image](https://github.com/user-attachments/assets/40aa6064-1203-4967-8135-a40feefa7b7c)

**DetalleVenta**

![image](https://github.com/user-attachments/assets/423ac2ef-fe39-43aa-828f-a97dbf43f371)

**Venta**

![image](https://github.com/user-attachments/assets/d3c992bd-b365-4fa8-8268-78d8c60899be)

## PARTE 4 - Programación DAL

**EmpleadoDAL**

![devenv_LjfbegSPIs](https://github.com/user-attachments/assets/b4b1d71b-ea9c-48ed-8904-e9d2c0af2b45)

**ClienteDAL**

![devenv_d80SJ3MqeN](https://github.com/user-attachments/assets/1c2d9d54-d81f-4d8d-9556-3e2edd5c04ec)

**ProductoDAL**

![devenv_afu9zsgzAZ](https://github.com/user-attachments/assets/4ad739b5-15d2-4543-bdf8-f6e2d1842a48)

**DetalleVentaDAL**

```csharp
        #region Metodos GUARDAR
        public static SqlCommand Guardar(DetalleVenta pDetalleVenta, SqlTransaction pTransaccion)
        {
            SqlCommand comando = ComunDB.ObtenerComando(pTransaccion);
            comando.CommandText = "SP_InsertarDetalleVenta";
            comando.CommandType = CommandType.StoredProcedure;
            comando.Parameters.AddWithValue("@IdVenta", pDetalleVenta.IdVenta);
            comando.Parameters.AddWithValue("@IdProducto", pDetalleVenta.IdProducto);
            comando.Parameters.AddWithValue("@Cantidad", pDetalleVenta.Cantidad);
            comando.Parameters.AddWithValue("@Precio", pDetalleVenta.Precio);
            comando.Parameters.AddWithValue("@Subtotal", pDetalleVenta.Subtotal);
            return comando;
        }
        #endregion
```

![devenv_O3uAA3z6VF](https://github.com/user-attachments/assets/c1fbd52d-48f4-485d-9233-ecbde90a118b)

