# TRATAMIENTO DE DATOS - BACKEND

Para mostrar correctamente los datos del Empleado, deberemos configurar el back-end.

![Tratamiento de datos](https://github.com/user-attachments/assets/602283aa-7b03-4b24-b47d-481b8f16271a)

Para esta implementacion, se quieren tratar los datos de la **CLASE B** que depende de la **CLASE A**, por ello crearemos un ***"nuevo metodo"*** para obtener un diccionario con los datos de la **CLASE A**, en su **archivo de clase DAL**.

![image](https://github.com/user-attachments/assets/6427ca0b-7ab0-40c1-bcd4-3c0ef0db3056)

## PARTE 1 - Configuracion de la DAL de CargoDAL

**Paso 1:** Ir al Explorador de soluciones y en proyecto “SistemaElParaisal.DAL”, seleccionar y dar 
doble clic sobre el archivo “CargoDAL.cs” para abrirlo. 

![image](https://github.com/user-attachments/assets/a6dca78d-b5db-4b61-9cdb-d109719fa003)

**Resultado:**
![image](https://github.com/user-attachments/assets/c2ddcc2b-fd44-49fe-8e3b-cf47a73e9267)

---

### ¿Qué es un Dictionary? [Leer más](https://oregoom.com/c-sharp/diccionarios/) 
Un diccionario es una colección de pares **clave-valor** en la que cada **clave** se asocia a un único **valor**. 

---

**Paso 2:** Ubicarse abajo del método Buscar y agregar el método público estatico **"ObtenerDiccionario"**.
```csharp
// TRATAMIENTO DE DATOS
public static Dictionary<byte, Cargo> ObtenerDiccionario(byte[] pArrayIdCargo)
{
    List<Cargo> lista = new List<Cargo>();

    // El return se puede explicar como: "x" es una instancia de cargo
    // y el metodo debe devolver un diccionario <byte, Cargo>, IdCargo es de tipo byte y Cargo es de tipo Cargo 
    return lista.ToDictionary(x => x.IdCargo, x => x);
}
```

![image](https://github.com/user-attachments/assets/dcc3e427-32e0-4e84-84b3-a44ed6eae720)

**Resultado:**

![image](https://github.com/user-attachments/assets/0a663ed9-73d6-4861-a13a-77b704293e05)

- En este caso para el diccionario, **IdCargo** es la **Clave** y **Cargo** es el **Valor**.
- **byte[] pArrayIdCargo**: es un parametro que contiene los Id's de los cargos que se desean obtener de la base de datos.

**Paso 3:** Codificar el metodo de acceso a datos, para Obtener los cargos por su id.

![image](https://github.com/user-attachments/assets/531229f8-fef4-4f91-863b-2d78fa81ec93)

```csharp
using (SqlCommand comando = ComunDB.ObtenerComando())
{
    string consulta = @"SELECT c.IdCargo, c.Nombre
                        FROM Cargo c ";
    // Agregar Array de Id 
    consulta += "WHERE IdCargo IN (" + string.Join(",", pArrayIdCargo) + ")";
    comando.CommandText = consulta;

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
```

### Metodo ObtenerDicccionario()
```csharp
// TRATAMIENTO DE DATOS
public static Dictionary<byte, Cargo> ObtenerDiccionario(byte[] pArrayIdCargo)
{
    List<Cargo> lista = new List<Cargo>();

    using (SqlCommand comando = ComunDB.ObtenerComando())
    {
        string consulta = @"SELECT c.IdCargo, c.Nombre
                            FROM Cargo c ";
        // Agregar Array de Id 
        consulta += "WHERE IdCargo IN (" + string.Join(",", pArrayIdCargo) + ")";
        comando.CommandText = consulta;

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
    // El return se puede explicar como: "x" es una instancia de cargo
    // y el metodo debe devolver un diccionario <byte, Cargo>, IdCargo es de tipo byte y Cargo es de tipo Cargo 
    return lista.ToDictionary(x => x.IdCargo, x => x);
}
```


## PARTE 2 - Configuracion de la BL de EmpleadoBL

En la seccion anterior se creo el metodo **"ObtenerDiccionario"** en **CargoDAL.cs**

Ahora vamos a configurar la logica para inyectar la ***Propiedad virtual de "Cargo"*** en **Empleado**, para ello configuraremos la clase en la **Capa BL**, trabajando en **EmpleadoBL**.

![image](https://github.com/user-attachments/assets/7e63e2bf-7d71-4b5e-b1aa-e2a8f4a69214)

---

**Paso 1:** Ir al Explorador de soluciones y en proyecto “SistemaElParaisal.BL”, seleccionar y dar 
doble clic sobre el archivo “EmpleadoBL.cs” para abrirlo. 

![image](https://github.com/user-attachments/assets/38d4a750-105b-4a63-ac25-71c537059b37)

**Resultado:**

![image](https://github.com/user-attachments/assets/02971f41-3cf0-405f-bdbb-6fce0b3a6920)

**Paso 2:** Ubicarse abajo del método **Buscar** y agregar el método público **"CargaCargoVirtual"**.

**Estructura:**
```
Cargar + Clase + Virtual
```

![image](https://github.com/user-attachments/assets/c36976c0-cd9d-4359-9d3f-77636ead352f)

```csharp
public void CargarCargoVirtual(List<Empleado> pLista, Action<List<Cargo>> pAccion = null)
{
    // Metodo para cargar los datos de la propiedad virtual de Cargo
    if (pLista.Count > 0)
    {

    }
}
```

![image](https://github.com/user-attachments/assets/3e25d595-e844-4e5b-a466-76954d8fd26c)


**Paso 3:** Extraer el **array de IdCargo** de la lista de empleados, y luego **crear un diccionario** mediante una busquedad de los cargos en la base de datos, en base a los id's del array.  

![image](https://github.com/user-attachments/assets/e5f4573b-18b0-45e2-9b4c-9b6189266b50)

```csharp
// Obtener array de ids de cargo de la lista de empleados
byte[] arrayIdCargo = pLista.Select(x => x.IdCargo).Distinct().ToArray();

// Crear Diccionario de Cargos buscando en la base de datos
// byte = Tipo de dato de la propiedad "IdCargo" en la clase Cargo
// Cargo = Instancia para guardar los datos de la clase Cargo
Dictionary<byte, Cargo> diccionario = CargoDAL.ObtenerDiccionario(arrayIdCargo);
```

**Paso 4:** Agregar un ciclo/bucle de tipo **foreach** para recorrer la lista donde se cargara su propiedad virtual desde el **Diccionario de Cargos**.

![image](https://github.com/user-attachments/assets/c2e1818a-0f96-4161-b850-68cdf1ecbd74)

```csharp
// Bucle para validar los Cargos e inyectarlo a los Empleados en su propiedad virtual
foreach (var item in pLista)
{
    // Verificar si existe el Cargo en el diccionario
    if (diccionario.ContainsKey(item.IdCargo) == true)
    {
        // Si existe, inyectar el Cargo desde el diccionario
        item.Cargo = diccionario[item.IdCargo];
    }
}
```

**Paso 5:** Agregar lógica para sobrecargar el método actual con un sub-método de tipo Action 
para obtener propiedades virtuales de tercer nivel. 

![image](https://github.com/user-attachments/assets/aea4aaf1-1286-420f-ac41-1f0573715516)

```csharp
// Accion auxiliar para sobrecargar otra propiedad virtual dentro de la clase Cargo
if (pAccion != null && diccionario.Count > 0)
{
    pAccion(diccionario.Select(x => x.Value).ToList());
}
```

![image](https://github.com/user-attachments/assets/18693b87-e188-4d3c-8cda-4d84a1338db8)

### Metodo CargarCargoVirtual()
```csharp
public void CargarCargoVirtual(List<Empleado> pLista, Action<List<Cargo>> pAccion = null)
{
    // Metodo para cargar los datos de la propiedad virtual de Cargo
    if (pLista.Count > 0)
    {
        // Obtener array de ids de cargo de la lista de empleados
        byte[] arrayIdCargo = pLista.Select(x => x.IdCargo).Distinct().ToArray();

        // Crear Diccionario de Cargos buscando en la base de datos
        // byte = Tipo de dato de la propiedad "IdCargo" en la clase Cargo
        // Cargo = Instancia para guardar los datos de la clase Cargo
        Dictionary<byte, Cargo> diccionario = CargoDAL.ObtenerDiccionario(arrayIdCargo);

        // Bucle para validar los Cargos e inyectarlo a los Empleados en su propiedad virtual
        foreach (var item in pLista)
        {
            // Verificar si existe el Cargo en el diccionario
            if (diccionario.ContainsKey(item.IdCargo) == true)
            {
                // Si existe, inyectar el Cargo desde el diccionario
                item.Cargo = diccionario[item.IdCargo];
            }
        }

        // Accion auxiliar para sobrecargar otra propiedad virtual dentro de la clase Cargo
        if (pAccion != null && diccionario.Count > 0)
        {
            pAccion(diccionario.Select(x => x.Value).ToList());
        }
    }
}
```

---

# CODIGO FUENTE

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

        // TRATAMIENTO DE DATOS
        public static Dictionary<byte, Cargo> ObtenerDiccionario(byte[] pArrayIdCargo)
        {
            List<Cargo> lista = new List<Cargo>();

            using (SqlCommand comando = ComunDB.ObtenerComando())
            {
                string consulta = @"SELECT c.IdCargo, c.Nombre
                                    FROM Cargo c ";
                // Agregar Array de Id 
                consulta += "WHERE IdCargo IN (" + string.Join(",", pArrayIdCargo) + ")";
                comando.CommandText = consulta;

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
            // El return se puede explicar como: "x" es una instancia de cargo
            // y el metodo debe devolver un diccionario <byte, Cargo>, IdCargo es de tipo byte y Cargo es de tipo Cargo 
            return lista.ToDictionary(x => x.IdCargo, x => x);
        }
    }
}
```

## Archivo **EmpleadoBL.cs**
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
//Referencias
using System.Security.Cryptography;
// Referencias del proyecto
using SistemaElParaisal.EN;
using SistemaElParaisal.DAL;

namespace SistemaElParaisal.BL
{
    public class EmpleadoBL
    {
        private static string CifrarHashSha256(string pTexto)
        {
            // Metodo para cifrar las claves
            byte[] bytes = Encoding.Unicode.GetBytes(pTexto);
            SHA256Managed hashstring = new SHA256Managed();
            byte[] hash = hashstring.ComputeHash(bytes);
            string hashString = string.Empty;
            foreach (byte x in hash)
            {
                hashString += String.Format("{0:x2}", x);
            }
            return hashString;
        }
        public int Guardar(Empleado pEmpleado)
        {
            pEmpleado.Clave = CifrarHashSha256(pEmpleado.Clave);
            return EmpleadoDAL.Guardar(pEmpleado);
        }
        public int Modificar(Empleado pEmpleado)
        {
            pEmpleado.Clave = CifrarHashSha256(pEmpleado.Clave);
            return EmpleadoDAL.Modificar(pEmpleado);
        }
        public int Eliminar(Empleado pEmpleado)
        {
            return EmpleadoDAL.Eliminar(pEmpleado);
        }
        public Empleado ObtenerPorId(short pIdEmpleado)
        {
            return EmpleadoDAL.ObtenerPorId(pIdEmpleado);
        }
        public List<Empleado> Buscar(Empleado pEmpleado)
        {
            return EmpleadoDAL.Buscar(pEmpleado);
        }

        public void CargarCargoVirtual(List<Empleado> pLista, Action<List<Cargo>> pAccion = null)
        {
            // Metodo para cargar los datos de la propiedad virtual de Cargo
            if (pLista.Count > 0)
            {
                // Obtener array de ids de cargo de la lista de empleados
                byte[] arrayIdCargo = pLista.Select(x => x.IdCargo).Distinct().ToArray();

                // Crear Diccionario de Cargos buscando en la base de datos
                // byte = Tipo de dato de la propiedad "IdCargo" en la clase Cargo
                // Cargo = Instancia para guardar los datos de la clase Cargo
                Dictionary<byte, Cargo> diccionario = CargoDAL.ObtenerDiccionario(arrayIdCargo);

                // Bucle para validar los Cargos e inyectarlo a los Empleados en su propiedad virtual
                foreach (var item in pLista)
                {
                    // Verificar si existe el Cargo en el diccionario
                    if (diccionario.ContainsKey(item.IdCargo) == true)
                    {
                        // Si existe, inyectar el Cargo desde el diccionario
                        item.Cargo = diccionario[item.IdCargo];
                    }
                }

                // Accion auxiliar para sobrecargar otra propiedad virtual dentro de la clase Cargo
                if (pAccion != null && diccionario.Count > 0)
                {
                    pAccion(diccionario.Select(x => x.Value).ToList());
                }
            }
        }

    }
}
```
