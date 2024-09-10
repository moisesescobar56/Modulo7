# CODIFICACION DE METODOS EN LA CAPA DAL 

## PARTE 1 - Analisis de metodos segun diagrama de clases
Es recomendable haber definido en algun documento (diagrama de clases) los metodos de acceso a datos correspondientes a cada clase de nuestro modelo de datos (tablas de la base de datos). Antes de iniciar debemos auxiliarnos en el diagrama de clases e identificar los metodos a crear para cada clase en la capa DAL.
![image](https://github.com/user-attachments/assets/b50c27ce-6abc-4d71-bbce-e5aa906673ca)

## PARTE 2 - Creacion de clases
Los metodos de nuestro modelo de datos debe crearse en la **"Capa de Acceso a Datos (DAL)"** como se muestra en el siguiente esquema: 

![image](https://github.com/user-attachments/assets/74309190-628f-4d7e-a059-af3364e6ea99)

Es importante respetar el orden de creacion del diagrama de clases. Para el ejemplo solo se trabajara con los metodos  de las clases **Cargo y Empleado**, pero en un escenario real se iran creando las clases segun sea necesario.

![image](https://github.com/user-attachments/assets/dcc9bfd9-6e92-4574-aef8-4fd7b8580d91)

**IMPORTANTE:** se recomienda primero crear los archivos de clases, establecerlas como **"public"** y luego codificarlas.

## PARTE 3 - Metodos CargoDAL
**Paso 1:** Ubicarse en la capa **"SistemaElParaisal.DAL"** y dar clic derecho y seleccionar **"Agregar > Clase"**.

![image](https://github.com/user-attachments/assets/830112ac-8266-4320-ad40-8fb331927f60)

**Paso 2:** Nombrar la clase **"CargoDAL.cs"** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/e7378ac6-1fa9-49c9-bb5c-a2caa3af6612)

**Paso 3:** Establecer como **"public"** la clase **"CargoDAL.cs"** y **Guardar** los cambios.

![image](https://github.com/user-attachments/assets/737a1774-15fd-4304-ac36-688c7cbeb770)

**Paso 4:** Agregar en la seccion de using las referencias a las bibliotecas de acceso a datos a utiizar.

```csharp
// Referencias
using System.Data;
using System.Data.SqlClient;
// Referencias del proyecto
using SistemaElParaisal.EN;
```
**Resultado:**
![image](https://github.com/user-attachments/assets/b06c088c-28c1-45ec-92ad-7ac885493520)

**Paso 5:** Identificar los metodos de **CargoDAL** segun el diagrama de clases y **Guardar** los cambios.

![image](https://github.com/user-attachments/assets/4ee01977-4ec0-4b54-aa1d-5298ad77d3d6)

**Paso 6:** Codificar el metodo **"ObtenerPorId"** en **CargoDAL** y **Guardar** los cambios.

```csharp
#region Metodos de Busqueda
public static Cargo ObtenerPorId(byte pIdCargo)
{
    Cargo obj = new Cargo();

    SqlCommand comando = ComunDB.ObtenerComando();
    comando.CommandType = CommandType.StoredProcedure;
    comando.CommandText = "SP_ObtenerCargoPorId";
    comando.Parameters.AddWithValue("@IdCargo", pIdCargo);

    SqlDataReader reader = ComunDB.EjecutarComandoReader(comando);
    while (reader.Read())
    {
        // Orden de las columnas depende de la Consulta SELECT utilizada
        obj.IdCargo = reader.GetByte(0); // Columna [0] cero
        obj.Nombre = reader.GetString(1);  // Columna [1] uno
    }
    return obj;
}
#endregion
```
**NOTA:** Los parametros deben ir en el mismo orden y con el mismo nombre del procedimiento almacenado en el gestor de SQL Server.

![image](https://github.com/user-attachments/assets/f45422b8-a289-48f6-8dc5-64b2f57ae903)

**Resultado:**
![image](https://github.com/user-attachments/assets/f66983b5-f02d-4f29-aec4-0087988475e6)

- **SqlCommand:** clase que permite crear un objeto para enviar una consulta SQL a la base de datos establecida.
- **CommandType** propiedad que permite establecer el tipo de consulta SQL a ejecutar ***Text*** o ***StoredProcedure*** 
- **Parameters.AddWithValue:** metodo que permite agregar los parametros dentro de la consulta SQL.
- **SqlDataReader:** clase que permite crear un objeto para ejecutar una consulta SQL de tipo SELECT y leer el resultado.
- **GetByte:** metodo para leer un dato de tipo byte (tinyint)
- **GetString:** metodo para leer un dato de tipo string (varchar)

**Paso 6:** Codificar el metodo **"Buscar"** en **CargoDAL** y **Guardar** los cambios.

```csharp
public static List<Cargo> Buscar(Cargo pCargo)
{
    List<Cargo> lista = new List<Cargo>();

    #region Proceso
    using (SqlCommand comando = ComunDB.ObtenerComando())
    {
        byte contador = 0;
        string whereSQL = " ";
        string consulta = @"SELECT TOP 100 c.IdCargo, c.Nombre
                            FROM Cargo c ";

        // Validar filtros
        if (pCargo.Nombre != null && pCargo.Nombre.Trim() != string.Empty)
        {
            if (contador > 0)
                whereSQL += " AND ";
            contador += 1;
            whereSQL += " c.Nombre LIKE @Nombre ";
            comando.Parameters.AddWithValue("@Nombre", "%" + pCargo.Nombre + "%");
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
            Cargo obj = new Cargo();
            // Orden de las columnas depende de la Consulta SELECT utilizada
            obj.IdCargo = reader.GetByte(0);
            obj.Nombre = reader.GetString(1);
            lista.Add(obj);
        }
        comando.Connection.Dispose();
    }
    #endregion
    return lista;
}
```

**Resultado:**
![image](https://github.com/user-attachments/assets/17ccda30-698c-4a99-898d-d0effd3ac7e0)

## PARTE 4 - Metodos EmpleadoDAL
**Paso 1:** Ubicarse en la capa **"SistemaElParaisal.DAL"** y dar clic derecho y seleccionar **"Agregar > Clase"**.

![image](https://github.com/user-attachments/assets/830112ac-8266-4320-ad40-8fb331927f60)

**Paso 2:** Nombrar la clase **"EmpleadoDAL.cs"** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/a1a1587c-c89c-45b9-af85-fb68174cac9d)

**Paso 3:** Establecer como **"public"** la clase **"EmpleadoDAL.cs"** y **Guardar** los cambios.

![image](https://github.com/user-attachments/assets/456ee339-1595-4840-877e-2c2e7dac19be)

**Paso 4:** Agregar en la seccion de using las referencias a las bibliotecas de acceso a datos a utiizar.

```csharp
// Referencias
using System.Data;
using System.Data.SqlClient;
// Referencias del proyecto
using SistemaElParaisal.EN;
```
**Resultado:**
![image](https://github.com/user-attachments/assets/ff7b3897-2766-4f46-ab10-031c66c17763)

**Paso 5:** Identificar los metodos de **CargoDAL** segun el diagrama de clases y **Guardar** los cambios.

![image](https://github.com/user-attachments/assets/f5f35585-645f-45aa-8347-88327f4dc4ad)

**Paso 6:** Codificar el metodo **"Guardar"** en **EmpleadoDAL** y **Guardar** los cambios.
```csharp
#region Metodos GUARDAR, MODIFICAR Y ELIMINAR
public static int Guardar(Empleado pEmpleado)
{
    SqlCommand comando = ComunDB.ObtenerComando();
    comando.CommandText = "SP_InsertarEmpleado";
    comando.CommandType = CommandType.StoredProcedure;
    comando.Parameters.AddWithValue("@IdCargo", pEmpleado.IdCargo);
    comando.Parameters.AddWithValue("@Nombre", pEmpleado.Nombre);
    comando.Parameters.AddWithValue("@Apellido", pEmpleado.Apellido);
    comando.Parameters.AddWithValue("@Telefono", pEmpleado.Telefono);
    comando.Parameters.AddWithValue("@Clave", pEmpleado.Clave);
    return ComunDB.EjecutarComando(comando);
}
#endregion
```
**NOTA:** Los parametros deben ir en el mismo orden y con el mismo nombre del procedimiento almacenado en el gestor de SQL Server.

![image](https://github.com/user-attachments/assets/6eaf015a-d092-4d83-9176-6c92396badc6)

- **SqlCommand:** clase que permite crear un objeto para enviar una consulta SQL a la base de datos establecida.
- **CommandType** propiedad que permite establecer el tipo de consulta SQL a ejecutar ***Text*** o ***StoredProcedure*** 
- **Parameters.AddWithValue:** metodo que permite agregar los parametros dentro de la consulta SQL.

**Resultado:**
![image](https://github.com/user-attachments/assets/a13360d7-0e7b-49c7-b3f1-583cae8a2c95)

**Paso 7:** Codificar el metodo **"Modificar"** en **EmpleadoDAL** y **Guardar** los cambios.
```csharp
public static int Modificar(Empleado pEmpleado)
{
    SqlCommand comando = ComunDB.ObtenerComando();
    comando.CommandText = "SP_ModificarEmpleado";
    comando.CommandType = CommandType.StoredProcedure;
    comando.Parameters.AddWithValue("@IdEmpleado", pEmpleado.IdEmpleado);
    comando.Parameters.AddWithValue("@IdCargo", pEmpleado.IdCargo);
    comando.Parameters.AddWithValue("@Nombre", pEmpleado.Nombre);
    comando.Parameters.AddWithValue("@Apellido", pEmpleado.Apellido);
    comando.Parameters.AddWithValue("@Telefono", pEmpleado.Telefono);
    comando.Parameters.AddWithValue("@Clave", pEmpleado.Clave);
    return ComunDB.EjecutarComando(comando);
}
```
**NOTA:** Los parametros deben ir en el mismo orden y con el mismo nombre del procedimiento almacenado en el gestor de SQL Server.

![image](https://github.com/user-attachments/assets/21704618-56a2-4e3e-947c-ce73f4db0aa8)

**Resultado:**
![image](https://github.com/user-attachments/assets/99e75be6-62d7-4204-b360-816cf53e2974)

**Paso 8:** Codificar el metodo **"Eliminar"** en **EmpleadoDAL** y **Guardar** los cambios.
```csharp
public static int Eliminar(Empleado pEmpleado)
{
    SqlCommand comando = ComunDB.ObtenerComando();
    comando.CommandText = "SP_EliminarEmpleado";
    comando.CommandType = CommandType.StoredProcedure;
    comando.Parameters.AddWithValue("@IdEmpleado", pEmpleado.IdEmpleado);
    return ComunDB.EjecutarComando(comando);
}
```
**NOTA:** Los parametros deben ir en el mismo orden y con el mismo nombre del procedimiento almacenado en el gestor de SQL Server.

![image](https://github.com/user-attachments/assets/47a48ad2-2532-49c8-8e4d-5c2848a1d84f)

**Resultado:**
![image](https://github.com/user-attachments/assets/8f76d505-a433-4cbb-a015-7ee5a000aa2f)

**Paso 8:** Codificar el metodo **"ObtenerPorId"** en **EmpleadoDAL** y **Guardar** los cambios.
```csharp
#region Metodos de Busqueda
public static Empleado ObtenerPorId(short pIdEmpleado)
{
    Empleado obj = new Empleado();

    SqlCommand comando = ComunDB.ObtenerComando();
    comando.CommandType = CommandType.StoredProcedure;
    comando.CommandText = "SP_ObtenerEmpleadoPorId";
    comando.Parameters.AddWithValue("@IdEmpleado", pIdEmpleado);

    SqlDataReader reader = ComunDB.EjecutarComandoReader(comando);
    while (reader.Read())
    {
        // Orden de las columnas depende de la Consulta SELECT utilizada
        obj.IdEmpleado = reader.GetInt16(0); // Columna [0] cero
        obj.IdCargo = reader.GetByte(1); // Columna [0] cero
        obj.Nombre = reader.GetString(2);  // Columna [1] uno
        obj.Apellido = reader.GetString(3); // Columna [2] dos
        obj.Telefono = reader.GetString(4); // Columna [4] cuatro
    }
    return obj;
}
#endregion
```
**NOTA:** Los parametros deben ir en el mismo orden y con el mismo nombre del procedimiento almacenado en el gestor de SQL Server.

![image](https://github.com/user-attachments/assets/c1dbcd87-9475-414f-af3a-b944aef26eca)

**Resultado:**
![image](https://github.com/user-attachments/assets/540e788a-8ac4-4bf2-9857-3f8c50285ab3)

- **SqlCommand:** clase que permite crear un objeto para enviar una consulta SQL a la base de datos establecida.
- **CommandType** propiedad que permite establecer el tipo de consulta SQL a ejecutar ***Text*** o ***StoredProcedure*** 
- **Parameters.AddWithValue:** metodo que permite agregar los parametros dentro de la consulta SQL.
- **SqlDataReader:** clase que permite crear un objeto para ejecutar una consulta SQL de tipo SELECT y leer el resultado.
- **GetInt16:** metodo para leer un dato de tipo short (smallint)
- **GetByte:** metodo para leer un dato de tipo byte (tinyint)
- **GetString:** metodo para leer un dato de tipo string (varchar)

**Paso 9:** Codificar el metodo **"Buscar"** en **EmpleadoDAL** y **Guardar** los cambios.
```csharp
public static List<Empleado> Buscar(Empleado pEmpleado)
{
    List<Empleado> lista = new List<Empleado>();

    #region Proceso
    using (SqlCommand comando = ComunDB.ObtenerComando())
    {
        byte contador = 0;
        string whereSQL = " ";
        string consulta = @"SELECT TOP 100 e.IdEmpleado, e.IdCargo, e.Nombre, e.Apellido, e.Telefono
                            FROM Empleado e ";

        // Validar filtros
        if (pEmpleado.IdCargo > 0)
        {
            if (contador > 0)
                whereSQL += " AND ";
            contador += 1;
            whereSQL += " e.IdCargo = @IdCargo ";
            comando.Parameters.AddWithValue("@IdCargo", pEmpleado.IdCargo);
        }
        if (pEmpleado.Nombre != null && pEmpleado.Nombre.Trim() != string.Empty)
        {
            if (contador > 0)
                whereSQL += " AND ";
            contador += 1;
            // @ValorNA = Valor Nombre/Apellido
            whereSQL += " (e.Nombre LIKE @ValorNA OR e.Apellido LIKE @ValorNA) ";
            comando.Parameters.AddWithValue("@ValorNA", "%" + pEmpleado.Nombre + "%");
        }
        if (pEmpleado.Telefono != null && pEmpleado.Telefono.Trim() != string.Empty)
        {
            if (contador > 0)
                whereSQL += " AND ";
            contador += 1;
            whereSQL += " e.Telefono = @Telefono ";
            comando.Parameters.AddWithValue("@Telefono", pEmpleado.Telefono);
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
            Empleado obj = new Empleado();
            // Orden de las columnas depende de la Consulta SELECT utilizada
            obj.IdEmpleado = reader.GetInt16(0); // Columna [0] cero
            obj.IdCargo = reader.GetByte(1); // Columna [0] cero
            obj.Nombre = reader.GetString(2);  // Columna [1] uno
            obj.Apellido = reader.GetString(3); // Columna [2] dos
            obj.Telefono = reader.GetString(4); // Columna [4] cuatro
            lista.Add(obj);
        }
        comando.Connection.Dispose();
    }
    #endregion

    return lista;
}
```
**Resultado:**
![image](https://github.com/user-attachments/assets/f5522209-3cb9-45d3-94f1-2a52c96e455a)

### **NOTA:** Al iniciar un proyecto es recomendable crear primero los archivos con accesibilidad publica y luego codificar los metodos. 

## Archivo **CargoDAL.cs**
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
    public class CargoDAL
    {
        #region Metodos de Busqueda
        public static Cargo ObtenerPorId(byte pIdCargo)
        {
            Cargo obj = new Cargo();

            SqlCommand comando = ComunDB.ObtenerComando();
            comando.CommandType = CommandType.StoredProcedure;
            comando.CommandText = "SP_ObtenerCargoPorId";
            comando.Parameters.AddWithValue("@IdCargo", pIdCargo);

            SqlDataReader reader = ComunDB.EjecutarComandoReader(comando);
            while (reader.Read())
            {
                // Orden de las columnas depende de la Consulta SELECT utilizada
                obj.IdCargo = reader.GetByte(0); // Columna [0] cero
                obj.Nombre = reader.GetString(1);  // Columna [1] uno
            }
            return obj;
        }

        public static List<Cargo> Buscar(Cargo pCargo)
        {
            List<Cargo> lista = new List<Cargo>();

            #region Proceso
            using (SqlCommand comando = ComunDB.ObtenerComando())
            {
                byte contador = 0;
                string whereSQL = " ";
                string consulta = @"SELECT TOP 100 c.IdCargo, c.Nombre
                                    FROM Cargo c ";

                // Validar filtros
                if (pCargo.Nombre != null && pCargo.Nombre.Trim() != string.Empty)
                {
                    if (contador > 0)
                        whereSQL += " AND ";

                    contador += 1;
                    // @ValorNA = Valor Nombre/Apellido
                    whereSQL += " c.Nombre LIKE @Nombre ";
                    comando.Parameters.AddWithValue("@Nombre", "%" + pCargo.Nombre + "%");
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
                    Cargo obj = new Cargo();
                    // Orden de las columnas depende de la Consulta SELECT utilizada
                    obj.IdCargo = reader.GetByte(0);
                    obj.Nombre = reader.GetString(1);
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

## Archivo **EmpleadoDAL.cs**
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
    public class EmpleadoDAL
    {
        #region Metodos GUARDAR, MODIFICAR Y ELIMINAR
        public static int Guardar(Empleado pEmpleado)
        {
            SqlCommand comando = ComunDB.ObtenerComando();
            comando.CommandText = "SP_InsertarEmpleado";
            comando.CommandType = CommandType.StoredProcedure;
            comando.Parameters.AddWithValue("@IdCargo", pEmpleado.IdCargo);
            comando.Parameters.AddWithValue("@Nombre", pEmpleado.Nombre);
            comando.Parameters.AddWithValue("@Apellido", pEmpleado.Apellido);
            comando.Parameters.AddWithValue("@Telefono", pEmpleado.Telefono);
            comando.Parameters.AddWithValue("@Clave", pEmpleado.Clave);
            return ComunDB.EjecutarComando(comando);
        }
        
        public static int Modificar(Empleado pEmpleado)
        {
            SqlCommand comando = ComunDB.ObtenerComando();
            comando.CommandText = "SP_ModificarEmpleado";
            comando.CommandType = CommandType.StoredProcedure;
            comando.Parameters.AddWithValue("@IdEmpleado", pEmpleado.IdEmpleado);
            comando.Parameters.AddWithValue("@IdCargo", pEmpleado.IdCargo);
            comando.Parameters.AddWithValue("@Nombre", pEmpleado.Nombre);
            comando.Parameters.AddWithValue("@Apellido", pEmpleado.Apellido);
            comando.Parameters.AddWithValue("@Telefono", pEmpleado.Telefono);
            comando.Parameters.AddWithValue("@Clave", pEmpleado.Clave);
            return ComunDB.EjecutarComando(comando);
        }

        public static int Eliminar(Empleado pEmpleado)
        {
            SqlCommand comando = ComunDB.ObtenerComando();
            comando.CommandText = "SP_EliminarEmpleado";
            comando.CommandType = CommandType.StoredProcedure;
            comando.Parameters.AddWithValue("@IdEmpleado", pEmpleado.IdEmpleado);
            return ComunDB.EjecutarComando(comando);
        }
        #endregion

        #region Metodos de Busqueda
        public static Empleado ObtenerPorId(int pIdEmpleado)
        {
            Empleado obj = new Empleado();

            SqlCommand comando = ComunDB.ObtenerComando();
            comando.CommandType = CommandType.StoredProcedure;
            comando.CommandText = "SP_ObtenerEmpleadoPorId";
            comando.Parameters.AddWithValue("@IdEmpleado", pIdEmpleado);

            SqlDataReader reader = ComunDB.EjecutarComandoReader(comando);
            while (reader.Read())
            {
                // Orden de las columnas depende de la Consulta SELECT utilizada
                obj.IdEmpleado = reader.GetInt16(0); // Columna [0] cero
                obj.IdCargo = reader.GetByte(1); // Columna [0] cero
                obj.Nombre = reader.GetString(2);  // Columna [1] uno
                obj.Apellido = reader.GetString(3); // Columna [2] dos
                obj.Telefono = reader.GetString(4); // Columna [4] cuatro
            }
            return obj;
        }

        public static List<Empleado> Buscar(Empleado pEmpleado)
        {
            List<Empleado> lista = new List<Empleado>();

            #region Proceso
            using (SqlCommand comando = ComunDB.ObtenerComando())
            {
                byte contador = 0;
                string whereSQL = " ";
                string consulta = @"SELECT TOP 100 e.IdEmpleado, e.IdCargo, e.Nombre, e.Apellido, e.Telefono
                                    FROM Empleado e ";

                // Validar filtros
                if (pEmpleado.IdCargo > 0)
                {
                    if (contador > 0)
                        whereSQL += " AND ";

                    contador += 1;
                    whereSQL += " e.IdCargo = @IdCargo ";
                    comando.Parameters.AddWithValue("@IdCargo", pEmpleado.IdCargo);
                }
                if (pEmpleado.Nombre != null && pEmpleado.Nombre.Trim() != string.Empty)
                {
                    if (contador > 0)
                        whereSQL += " AND ";

                    contador += 1;
                    // @ValorNA = Valor Nombre/Apellido
                    whereSQL += " (e.Nombre LIKE @ValorNA OR e.Apellido LIKE @ValorNA) ";
                    comando.Parameters.AddWithValue("@ValorNA", "%" + pEmpleado.Nombre + "%");
                }
                if (pEmpleado.Telefono != null && pEmpleado.Telefono.Trim() != string.Empty)
                {
                    if (contador > 0)
                        whereSQL += " AND ";

                    contador += 1;
                    whereSQL += " e.Telefono = @Telefono ";
                    comando.Parameters.AddWithValue("@Telefono", pEmpleado.Telefono);
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
                    Empleado obj = new Empleado();
                    // Orden de las columnas depende de la Consulta SELECT utilizada
                    obj.IdEmpleado = reader.GetInt16(0); // Columna [0] cero
                    obj.IdCargo = reader.GetByte(1); // Columna [0] cero
                    obj.Nombre = reader.GetString(2);  // Columna [1] uno
                    obj.Apellido = reader.GetString(3); // Columna [2] dos
                    obj.Telefono = reader.GetString(4); // Columna [4] cuatro
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
