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

### **EmpleadoDAL**

![image](https://github.com/user-attachments/assets/199c947b-6d88-4228-b7e3-0d8908dc9bb9)

### **ClienteDAL**

![image](https://github.com/user-attachments/assets/a8f8fd96-e92e-4421-97b2-41d3db1591a9)

### Categoria DAL

![image](https://github.com/user-attachments/assets/e986758b-18eb-4fd3-84bf-06401189b7d7)

###**ProductoDAL**

![image](https://github.com/user-attachments/assets/4d2633fb-ebb6-4bf5-ac8e-719e74fc11e0)

### **DetalleVentaDAL**

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

![image](https://github.com/user-attachments/assets/4ad75772-303a-436d-afc2-79b3c74ff734)

### VentaDAL

```csharp
public static int Guardar(Venta pVenta)
{
    // Crear transaccion
    SqlTransaction transaccion = ComunDB.CrearTransaction();

    try
    {
	SqlCommand comando = ComunDB.ObtenerComando(transaccion);
	comando.CommandText = "SP_InsertarVenta";
	comando.CommandType = CommandType.StoredProcedure;
	comando.Parameters.AddWithValue("@IdEmpleado", pVenta.IdEmpleado);
	comando.Parameters.AddWithValue("@IdCliente", pVenta.IdCliente);
	comando.Parameters.AddWithValue("@Total", pVenta.Total);

	// Guardar Venta y obtener Id de la venta insertada
	long idVenta = Convert.ToInt64(comando.ExecuteScalar());

	foreach (var detalle in pVenta.DetallesVenta)
	{
	    // detalle: representa al producto que se agrego a la venta
	    detalle.IdVenta = idVenta;
	    SqlCommand comandoDetalle = DetalleVentaDAL.Guardar(detalle, transaccion);

	    // Confirmar Guardar detalle de la venta
	    comandoDetalle.ExecuteNonQuery();
	}

	// Confirmar cambios en la DB
	transaccion.Commit();
    }
    catch (Exception ex)
    {
	// Revertir cambios en la DB
	transaccion.Rollback();
	return 0;
    }
    finally
    {
	transaccion.Dispose();
    }

    // Venta guardada correctamente
    return 1;
}
```

```csharp
public static int Modificar(Venta pVenta)
{
    SqlCommand comando = ComunDB.ObtenerComando();
    comando.CommandText = "SP_ModificarVenta";
    comando.CommandType = CommandType.StoredProcedure;
    comando.Parameters.AddWithValue("@IdVenta", pVenta.IdVenta);
    comando.Parameters.AddWithValue("@Estatus", pVenta.Estatus);
    return ComunDB.EjecutarComando(comando);
}
```

![image](https://github.com/user-attachments/assets/9f788509-e116-47f2-990c-1d61fd861d6c)


## PARTE 5 - Programación BL

### EmpleadoBL

![image](https://github.com/user-attachments/assets/e6953df4-8e26-4ea6-96d9-2322d14a638d)

### ClienteBL

![image](https://github.com/user-attachments/assets/a9b4edd6-a5e4-4d4c-80d4-420f542318fd)

### CategoriaBL

![image](https://github.com/user-attachments/assets/fd628aed-19fa-4974-b977-95ace1cc840a)

### ProductoBL

![image](https://github.com/user-attachments/assets/b8aec25b-8c38-4bd0-9ff2-9f6b08c71e3d)

### DetalleVentaBL

![image](https://github.com/user-attachments/assets/30e1beb4-00fc-47f0-ac34-b7a29cc2fca1)

### VentaBL

![image](https://github.com/user-attachments/assets/2a86a550-8712-447f-8373-a49b5ba1b491)

# ARCHIVOS

## Venta.cs
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
// Referencias
using System.ComponentModel.DataAnnotations;

namespace SistemaElParaisal.EN
{
    public class Venta
    {
        [Display(Name = "Id")]
        public long IdVenta { get; set; }

        [Display(Name = "Empleado")]
        public short IdEmpleado { get; set; }

        [Required]
        [Display(Name = "Cliente")]
        public int IdCliente { get; set; }

        public long Correlativo { get; set; }

        [Display(Name = "Fecha Hora")]
        public DateTime FechaHora { get; set; }

        public decimal Total { get; set; }

        public byte Estatus { get; set; }

        public virtual Empleado Empleado { get; set; }
        public virtual Cliente Cliente { get; set; }

        // Lista de detalles vinculados a la venta
        public List<DetalleVenta> DetallesVenta { get; set; }

        // Enum para gestionar estatus de la venta
        public enum EstatusEnum
        {
            NINGUNO = 0,
            FACTURADA = 1,
            ANULADA = 2
        }
    }
}

```

## DetalleVenta.cs
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace SistemaElParaisal.EN
{
    public class DetalleVenta
    {
        public long IdDetalleVenta { get; set; }
        public long IdVenta { get; set; }
        public int IdProducto { get; set; }
        public decimal Precio { get; set; }
        public short Cantidad { get; set; }
        public decimal Subtotal { get; set; }

        public virtual Producto Producto { get; set; }
    }
}

```

## VentaDAL.cs
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
// Referencias
using System.Data;
using System.Data.SqlClient;
// Referencias del proyecto
using SistemaElParaisal.EN;

namespace SistemaElParaisal.DAL
{
    public class VentaDAL
    {
        #region Metodos GUARDAR Y MODIFICAR
        public static int Guardar(Venta pVenta)
        {
            // Crear transaccion
            SqlTransaction transaccion = ComunDB.CrearTransaction();

            try
            {
                SqlCommand comando = ComunDB.ObtenerComando(transaccion);
                comando.CommandText = "SP_InsertarVenta";
                comando.CommandType = CommandType.StoredProcedure;
                comando.Parameters.AddWithValue("@IdEmpleado", pVenta.IdEmpleado);
                comando.Parameters.AddWithValue("@IdCliente", pVenta.IdCliente);
                comando.Parameters.AddWithValue("@Total", pVenta.Total);

                // Guardar Venta y obtener Id de la venta insertada
                long idVenta = Convert.ToInt64(comando.ExecuteScalar());

                foreach (var detalle in pVenta.DetallesVenta)
                {
                    // detalle: representa al producto que se agrego a la venta
                    detalle.IdVenta = idVenta;
                    SqlCommand comandoDetalle = DetalleVentaDAL.Guardar(detalle, transaccion);

                    // Confirmar Guardar detalle de la venta
                    comandoDetalle.ExecuteNonQuery();
                }

                // Confirmar cambios en la DB
                transaccion.Commit();
            }
            catch (Exception ex)
            {
                // Revertir cambios en la DB
                transaccion.Rollback();
                return 0;
            }
            finally
            {
                transaccion.Dispose();
            }

            // Venta guardada correctamente
            return 1;
        }

        public static int Modificar(Venta pVenta)
        {
            SqlCommand comando = ComunDB.ObtenerComando();
            comando.CommandText = "SP_ModificarVenta";
            comando.CommandType = CommandType.StoredProcedure;
            comando.Parameters.AddWithValue("@IdVenta", pVenta.IdVenta);
            comando.Parameters.AddWithValue("@Estatus", pVenta.Estatus);
            return ComunDB.EjecutarComando(comando);
        }

        #endregion

        #region Metodos de Busqueda
        public static Venta ObtenerPorId(long pIdVenta)
        {
            Venta obj = new Venta();

            SqlCommand comando = ComunDB.ObtenerComando();
            comando.CommandType = CommandType.StoredProcedure;
            comando.CommandText = "SP_ObtenerVentaPorId";
            comando.Parameters.AddWithValue("@IdVenta", pIdVenta);

            SqlDataReader reader = ComunDB.EjecutarComandoReader(comando);
            while (reader.Read())
            {
                // Orden de las columnas depende de la Consulta SELECT utilizada
                obj.IdVenta = reader.GetInt64(0); // Columna [0] cero
                obj.IdEmpleado = reader.GetInt16(1);  // Columna [1] uno
                obj.IdCliente = reader.GetInt32(2);  // Columna [2] dos
                obj.Correlativo = reader.GetInt64(3);  // Columna [3] tres
                obj.FechaHora = reader.GetDateTime(4);  // Columna [4] cuatro
                obj.Total = reader.GetDecimal(5);  // Columna [5] cinco
                obj.Estatus = reader.GetByte(6);  // Columna [6] seis
            }
            return obj;
        }

        public static List<Venta> Buscar(Venta pVenta)
        {
            List<Venta> lista = new List<Venta>();

            #region Proceso
            using (SqlCommand comando = ComunDB.ObtenerComando())
            {
                byte contador = 0;
                string whereSQL = " ";
                string consulta = @"SELECT TOP 100 v.IdVenta, v.IdEmpleado, v.IdCliente, v.Correlativo, v.FechaHora, v.Total, v.Estatus
                                    FROM Venta v ";

                // Validar filtros
                if (pVenta.IdEmpleado > 0)
                {
                    if (contador > 0)
                        whereSQL += " AND ";

                    contador += 1;
                    whereSQL += " v.IdEmpleado = @IdEmpleado ";
                    comando.Parameters.AddWithValue("@IdEmpleado", pVenta.IdEmpleado);
                }
                if (pVenta.IdCliente > 0)
                {
                    if (contador > 0)
                        whereSQL += " AND ";

                    contador += 1;
                    whereSQL += " v.IdCliente = @IdCliente ";
                    comando.Parameters.AddWithValue("@IdCliente", pVenta.IdCliente);
                }
                if (pVenta.Correlativo > 0)
                {
                    if (contador > 0)
                        whereSQL += " AND ";

                    contador += 1;
                    whereSQL += " v.Correlativo = @Correlativo ";
                    comando.Parameters.AddWithValue("@Correlativo", pVenta.Correlativo);
                }
                if (pVenta.FechaHora != null && pVenta.FechaHora != DateTime.MinValue)
                {
                    if (contador > 0)
                        whereSQL += " AND ";

                    contador += 1;
                    whereSQL += " v.FechaHora BETWEEN @FechaHoraIni AND @FechaHoraFin ";

                    // Agregar parametros para buscar ventas del dia completo 24horas
                    DateTime fechaHoraIni = pVenta.FechaHora.Date + new TimeSpan(0, 0, 0);
                    DateTime fechaHoraFin = pVenta.FechaHora.Date + new TimeSpan(23, 59, 59);

                    comando.Parameters.AddWithValue("@FechaHoraIni", fechaHoraIni);
                    comando.Parameters.AddWithValue("@FechaHoraFin", fechaHoraFin);
                }
                if (pVenta.Estatus > 0)
                {
                    if (contador > 0)
                        whereSQL += " AND ";

                    contador += 1;
                    whereSQL += " v.Estatus = @Estatus ";
                    comando.Parameters.AddWithValue("@Estatus", pVenta.Estatus);
                }

                // Agregar filtros
                if (whereSQL.Trim().Length > 0)
                {
                    whereSQL = " WHERE " + whereSQL;
                }
                comando.CommandText = consulta + whereSQL;

                SqlDataReader reader = ComunDB.EjecutarComandoReader(comando);
                while (reader.Read())
                {
                    Venta obj = new Venta();
                    // Orden de las columnas depende de la Consulta SELECT utilizada
                    obj.IdVenta = reader.GetInt64(0); // Columna [0] cero
                    obj.IdEmpleado = reader.GetInt16(1);  // Columna [1] uno
                    obj.IdCliente = reader.GetInt32(2);  // Columna [2] dos
                    obj.Correlativo = reader.GetInt64(3);  // Columna [3] tres
                    obj.FechaHora = reader.GetDateTime(4);  // Columna [4] cuatro
                    obj.Total = reader.GetDecimal(5);  // Columna [5] cinco
                    obj.Estatus = reader.GetByte(6);  // Columna [6] seis
                    lista.Add(obj);
                }
                comando.Connection.Dispose();

            }
            #endregion

            return lista;
        }
        #endregion
    }
}
```

## DetalleVentaDAL.cs
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
// Referencias
using System.Data;
using System.Data.SqlClient;
// Referencias del proyecto
using SistemaElParaisal.EN;

namespace SistemaElParaisal.DAL
{
    public class DetalleVentaDAL
    {
        #region Metodos GUARDAR, MODIFICAR Y ELIMINAR
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

        #region Metodos de Busqueda
        public static List<DetalleVenta> Buscar(DetalleVenta pDetalleVenta)
        {
            List<DetalleVenta> lista = new List<DetalleVenta>();

            #region Proceso
            using (SqlCommand comando = ComunDB.ObtenerComando())
            {
                byte contador = 0;
                string whereSQL = " ";
                string consulta = @"SELECT TOP 500 dt.IdDetalleVenta, dt.IdVenta, dt.IdProducto, dt.Cantidad, dt.Precio, dt.Subtotal
                                    FROM DetalleVenta dt ";

                // Validar filtros
                if (pDetalleVenta.IdVenta > 0)
                {
                    if (contador > 0)
                        whereSQL += " AND ";

                    contador += 1;
                    whereSQL += " dt.IdVenta = @IdVenta ";
                    comando.Parameters.AddWithValue("@IdVenta", pDetalleVenta.IdVenta);
                }
                if (pDetalleVenta.IdProducto > 0)
                {
                    if (contador > 0)
                        whereSQL += " AND ";

                    contador += 1;
                    whereSQL += " dt.IdProducto = @IdProducto ";
                    comando.Parameters.AddWithValue("@IdProducto", pDetalleVenta.IdProducto);
                }
                // Agregar filtros
                if (whereSQL.Trim().Length > 0)
                {
                    whereSQL = " WHERE " + whereSQL;
                }
                comando.CommandText = consulta + whereSQL;

                SqlDataReader reader = ComunDB.EjecutarComandoReader(comando);
                while (reader.Read())
                {
                    DetalleVenta obj = new DetalleVenta();
                    // Orden de las columnas depende de la Consulta SELECT utilizada
                    obj.IdDetalleVenta = reader.GetInt64(0); // Columna [0] cero
                    obj.IdVenta = reader.GetInt64(1);  // Columna [1] uno
                    obj.IdProducto = reader.GetInt32(2);  // Columna [2] dos
                    obj.Cantidad = reader.GetInt16(3);  // Columna [3] tres
                    obj.Precio = reader.GetDecimal(4);  // Columna [4] cuatro
                    obj.Subtotal = reader.GetDecimal(5);  // Columna [5] cinco
                    lista.Add(obj);
                }
                comando.Connection.Dispose();

            }
            #endregion

            return lista;
        }
        #endregion
    }
}
```

## VentaBL.cs
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
// Referencias del proyecto
using SistemaElParaisal.EN;
using SistemaElParaisal.DAL;

namespace SistemaElParaisal.BL
{
    public class VentaBL
    {
        public int Guardar(Venta pVenta)
        {
            return VentaDAL.Guardar(pVenta);
        }

        public int Anular(Venta pVenta)
        {
            pVenta.Estatus = (byte)Venta.EstatusEnum.ANULADA;
            return VentaDAL.Modificar(pVenta);
        }

        public Venta ObtenerPorId(long pIdVenta)
        {
            return VentaDAL.ObtenerPorId(pIdVenta);
        }

        public List<Venta> Buscar(Venta pVenta)
        {
            return VentaDAL.Buscar(pVenta);
        }

        public void CargarClienteVirtual(List<Venta> pLista, Action<List<Cliente>> pAccion = null)
        {
            // Metodo para cargar los datos de la propiedad virtual de Cliente
            if (pLista.Count > 0)
            {
                // Obtener array de ids de cliente de la lista de ventas
                int[] arrayIdCliente = pLista.Select(x => x.IdCliente).Distinct().ToArray();

                // Crear Diccionario de Clientes buscando en la base de datos
                // byte = Tipo de dato de la propiedad "IdCliente" en la clase Cliente
                // Cliente = Instancia para guardar los datos de la clase Cliente
                Dictionary<int, Cliente> diccionario = ClienteDAL.ObtenerDiccionario(arrayIdCliente);

                // Bucle para validar los Clientes e inyectarlo a los Ventas en su propiedad virtual
                foreach (var item in pLista)
                {
                    // Verificar si existe el Cliente en el diccionario
                    if (diccionario.ContainsKey(item.IdCliente) == true)
                    {
                        // Si existe, inyectar el Cliente desde el diccionario
                        item.Cliente = diccionario[item.IdCliente];
                    }
                }

                // Accion auxiliar para sobrecargar otra propiedad virtual dentro de la clase Cliente
                if (pAccion != null && diccionario.Count > 0)
                {
                    pAccion(diccionario.Select(x => x.Value).ToList());
                }
            }
        }

        public void CargarEmpleadoVirtual(List<Venta> pLista, Action<List<Empleado>> pAccion = null)
        {
            // Metodo para cargar los datos de la propiedad virtual de Empleado
            if (pLista.Count > 0)
            {
                // Obtener array de ids de empleado de la lista de ventas
                short[] arrayIdEmpleado = pLista.Select(x => x.IdEmpleado).Distinct().ToArray();

                // Crear Diccionario de Empleados buscando en la base de datos
                // byte = Tipo de dato de la propiedad "IdEmpleado" en la clase Empleado
                // Empleado = Instancia para guardar los datos de la clase Empleado
                Dictionary<short, Empleado> diccionario = EmpleadoDAL.ObtenerDiccionario(arrayIdEmpleado);

                // Bucle para validar los Empleados e inyectarlo a los Ventas en su propiedad virtual
                foreach (var item in pLista)
                {
                    // Verificar si existe el Empleado en el diccionario
                    if (diccionario.ContainsKey(item.IdEmpleado) == true)
                    {
                        // Si existe, inyectar el Empleado desde el diccionario
                        item.Empleado = diccionario[item.IdEmpleado];
                    }
                }

                // Accion auxiliar para sobrecargar otra propiedad virtual dentro de la clase Empleado
                if (pAccion != null && diccionario.Count > 0)
                {
                    pAccion(diccionario.Select(x => x.Value).ToList());
                }
            }
        }
    }
}
```

## DetalleVentaBL.cs
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
// Referencias del proyecto
using SistemaElParaisal.EN;
using SistemaElParaisal.DAL;

namespace SistemaElParaisal.BL
{
    public class DetalleVentaBL
    {
        public List<DetalleVenta> Buscar(DetalleVenta pDetalleVenta)
        {
            return DetalleVentaDAL.Buscar(pDetalleVenta);
        }

        public void CargarProductoVirtual(List<DetalleVenta> pLista, Action<List<Producto>> pAccion = null)
        {
            // Metodo para cargar los datos de la propiedad virtual de Producto
            if (pLista.Count > 0)
            {
                // Obtener array de ids de Producto de la lista de DetallesVenta
                int[] arrayIdProducto = pLista.Select(x => x.IdProducto).Distinct().ToArray();

                // Crear Diccionario de Productos buscando en la base de datos
                // byte = Tipo de dato de la propiedad "IdProducto" en la clase Producto
                // Producto = Instancia para guardar los datos de la clase Producto
                Dictionary<int, Producto> diccionario = ProductoDAL.ObtenerDiccionario(arrayIdProducto);

                // Bucle para validar los Productos e inyectarlo a los DetalleVentas en su propiedad virtual
                foreach (var item in pLista)
                {
                    // Verificar si existe el Producto en el diccionario
                    if (diccionario.ContainsKey(item.IdProducto) == true)
                    {
                        // Si existe, inyectar el Producto desde el diccionario
                        item.Producto = diccionario[item.IdProducto];
                    }
                }

                // Accion auxiliar para sobrecargar otra propiedad virtual dentro de la clase Producto
                if (pAccion != null && diccionario.Count > 0)
                {
                    pAccion(diccionario.Select(x => x.Value).ToList());
                }
            }
        }
    }
}
```
