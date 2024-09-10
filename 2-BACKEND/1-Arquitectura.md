# ARQUITECTURA DE N-CAPAS EN .NET
## PARTE 1 - Creacion de la arquitectura N-Capas del proyecto
**Paso 1:** Abrir Visual Studio y Crear un nuevo proyecto.

![image](https://github.com/user-attachments/assets/2bea8f03-9bc9-49ce-933e-f8e718fcff56)

**Paso 2:** Seleccionar Crear una solucion en blanco y dar clic en Siguiente.

![image](https://github.com/user-attachments/assets/118c92ee-78f1-4034-92e1-57a3d0681435)

**Paso 3:** Nombrar a la solucion en blanco y dar clic en Crear.
```
SistemaElParaisal
```
![image](https://github.com/user-attachments/assets/72dd07f4-2221-4bd3-8710-d478c4b8ba8e)

**Resultado:**
![image](https://github.com/user-attachments/assets/160602de-253a-4773-8372-a791a44492bb)

**Paso 4:** Dar clic derecho en la solucion y seleccionar **"Agregar > Nuevo proyecto"**. 

![image](https://github.com/user-attachments/assets/771e4004-c252-4187-ae31-dd4af2db70d1)

**Paso 5:** Seleccionar una **Biblioteca de clases (.NET Framework)** y dar clic en Siguiente.

![image](https://github.com/user-attachments/assets/87e1ae24-b91a-438a-be88-f5d240c95c21)

**Paso 6:** Nombrar a la **Biblioteca de clases (.NET Framework)** y dar clic en Crear.
```
SistemaElParaisal.EN
```

![image](https://github.com/user-attachments/assets/54f87cf6-99a6-4e38-b183-90734026eb6d)

**NOTA:** Eliminar el archivo Class1.cs siempre, solo aplicar para **Biblioteca de clases (.NET Framework)**
![image](https://github.com/user-attachments/assets/979b345e-670a-4b93-a154-2cec927a44c8)

**Paso 7:** Crear como **Biblioteca de clases (.NET Framework)** las capas:
- SistemaElParaisal.DAL
- SistemaElParaisal.BL

**Resultado:**
![image](https://github.com/user-attachments/assets/222880e5-db41-4416-b038-f99beeacd38c)

**Paso 8:** Dar clic derecho en la solucion y seleccionar **"Agregar > Nuevo proyecto"**. 

**Paso 9:** Seleccionar un **Proyecto de prueba unitaria(.NET Framework)** y dar clic en Siguiente.

![image](https://github.com/user-attachments/assets/632f3e8a-4a1a-4858-9a35-52b13f015e02)

**Paso 10:** Nombrar al **Proyecto de prueba unitaria(.NET Framework)** y dar clic en Crear.
```
SistemaElParaisal.PruebasUnitarias
```

![image](https://github.com/user-attachments/assets/2c2dcaca-d91e-4515-980c-f1f5b1d81468)

**NOTA:** Eliminar el archivo UnitTest1.cs siempre, solo aplicar para **Proyecto de prueba unitaria(.NET Framework)**

![image](https://github.com/user-attachments/assets/0a83c273-5a3c-4ad9-b2fa-250f386186bd)

**Resultado:**
![image](https://github.com/user-attachments/assets/8257b780-509a-4374-86b3-c82fc698620a)

**Paso 11:** Dar clic derecho en la solucion y seleccionar **"Agregar > Nuevo proyecto"**. 

**Paso 12:** Agregar a la solucion un proyecto de tipo **"Aplicacion de Windows Forms (.NET Framework)"**.
```
SistemaElParaisal.UI.WinForms
```

![image](https://github.com/user-attachments/assets/c2214f6a-4043-4fb7-b9e3-843172ae4fe4)

**Resultado:**
![image](https://github.com/user-attachments/assets/752aedc5-750b-430a-868f-e9ceb24f85b6)

## PARTE 2 - Configuracion de la arquitectura N-Capas del proyecto
En este proyecto nos basaremos en la arquitectura de N-Capas en la cual creamos capas independientes con una unica funcion. Para la configuracion nos baseremos en el siguiente esquema:

![2023 G2_G5 - Desarrollo de CRUD en aplicación de Windows Forms](https://github.com/user-attachments/assets/0509b3e1-024f-4fe8-a8d8-aeb478db355a)


**Paso 1:** Ubicarse en la capa **"SistemaElParaisal.DAL"** y en la opcion de **"Referencias"** dar clic derecho y seleccionar **"> Agregar referencia"**.

![image](https://github.com/user-attachments/assets/2cf46927-d789-44f8-ba98-86e0b29cd227)

**Paso 2:** Ubicarse en la opcion **Proyectos > Solución** y marcar la opcion **"SistemaElParaisal.EN"**.

![image](https://github.com/user-attachments/assets/884568aa-ccc2-4628-b506-c57d303425c7)

**Paso 3:** Revisar que quede marcada la opcion **"SistemaElParaisal.EN"** y dar clic en **Aceptar**.

![image](https://github.com/user-attachments/assets/81866b3a-7dfa-4811-a1a8-39a29c4a340e)

**Resultado:**
![image](https://github.com/user-attachments/assets/9a26324e-4522-468d-99a7-11c2801d694d)

**Paso 4:** Ubicarse en la capa **"SistemaElParaisal.BL"** y en la opcion de **"Referencias"** dar clic derecho y seleccionar **"> Agregar referencia"**.

![image](https://github.com/user-attachments/assets/822a08e9-8937-4228-853a-54c94cd0f30e)

**Paso 5:** Ubicarse en la opcion **Proyectos > Solución** y marcar las opciones **"SistemaElParaisal.EN"**  y **"SistemaElParaisal.DAL"**,

**Paso 6:** Revisar que quede marcada las opciones **"SistemaElParaisal.EN"**  y **"SistemaElParaisal.DAL"**, luego dar clic en **Aceptar**.

![image](https://github.com/user-attachments/assets/363e99e3-60c3-46b8-b333-b71775b4942e)


**Resultado:**
![image](https://github.com/user-attachments/assets/f535cf15-fad5-4e10-aaba-9fdec6a639b4)

**Paso 7:** Ubicarse en la capa **"SistemaElParaisal.PruebasUnitarias"** y en la opcion de **"Referencias"** dar clic derecho y seleccionar **"> Agregar referencia"**.

![image](https://github.com/user-attachments/assets/8fe5c5e8-933c-4371-be2b-2ef2ff6ae01a)

**Paso 8:** Ubicarse en la opcion **Proyectos > Solución** y marcar las opciones **"SistemaElParaisal.EN"**  y **"SistemaElParaisal.BL"**,

**Paso 9:** Revisar que quede marcada las opciones **"SistemaElParaisal.EN"**  y **"SistemaElParaisal.BL"**, luego dar clic en **Aceptar**.

![image](https://github.com/user-attachments/assets/c7a11c9b-f673-4d76-9a5d-dd0ba58ff69d)

**Resultado:**
![image](https://github.com/user-attachments/assets/2def6596-0a93-4295-ae88-767738c36c5e)

**Paso 10:** Ubicarse en la capa **"SistemaElParaisal.UI.WinForms"** y en la opcion de **"Referencias"** dar clic derecho y seleccionar **"> Agregar referencia"**.

![image](https://github.com/user-attachments/assets/fcf7f924-cb18-4e0f-b9c0-d0378d5738e0)

**Paso 11:** Ubicarse en la opcion **Proyectos > Solución** y marcar las opciones **"SistemaElParaisal.EN"**  y **"SistemaElParaisal.BL"**,

**Paso 12:** Revisar que quede marcada las opciones **"SistemaElParaisal.EN"**  y **"SistemaElParaisal.BL"**, luego dar clic en **Aceptar**.

![image](https://github.com/user-attachments/assets/19c80196-e377-4857-ab5c-81769b798a33)

**Resultado:**
![image](https://github.com/user-attachments/assets/26c7a597-c8b0-432b-b692-e5676a03caff)

## PARTE 3 - Configuracion de la comunDB
Para poder obtener acceso a la base de datos utilizando esta arquitectura, sera necesario crear un archivo comun que contenga metodos para conectarse a una base de datos, crear consultas y obtener los resultados. A continuacion se creara y configurara la clase ComunDB.

**Paso 1:** Ubicarse en la capa **"SistemaElParaisal.DAL"** y dar clic derecho y seleccionar **"Agregar > Clase"**.

![image](https://github.com/user-attachments/assets/f264c7fb-195b-43c3-a03f-b42bc0abde14)

**Paso 2:** Nombrar la clase **"ComunDB.cs"** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/be894ec9-9052-4585-9a65-24d27b8a4a46)

**Resultado:**
![image](https://github.com/user-attachments/assets/bca62f75-e4eb-47f0-9ffe-179c33d9d37a)

**Paso 3:** Establecer como **"public"** la clase **"ComunDB.cs"**

![image](https://github.com/user-attachments/assets/626de071-5dae-4384-ac90-8169fc14fdc7)

**Paso 4:** Agregar en la seccion de using las referencias a las bibliotecas de acceso a datos a utiizar.

```csharp
// Referencias
using System.Data;
using System.Data.SqlClient;
```

**Resultado:**
![image](https://github.com/user-attachments/assets/aa89eeb0-edde-46c8-98e5-245eb2a35bf7)

**Paso 5:** Agregar el String de conexion y metodos de acceso a datos.

```csharp
const string strConexion = @"Mi string de conexion aquí";

private static SqlConnection ObtenerConexion()
{
    SqlConnection conexion = new SqlConnection(strConexion);
    conexion.Open();
    return conexion;
}

public static SqlCommand ObtenerComando()
{
    SqlCommand comando = new SqlCommand();
    comando.Connection = ObtenerConexion();
    return comando;
}

public static SqlCommand ObtenerComando(SqlTransaction pTransaction)
{
    // Sobre carga del metodo ObtenerComando para crear comandos de transaccion
    SqlCommand comando = new SqlCommand();
    comando.Connection = pTransaction.Connection;
    comando.Transaction = pTransaction;
    return comando;
}

public static int EjecutarComando(SqlCommand pComando)
{
    int resultado = pComando.ExecuteNonQuery();
    pComando.Connection.Close();
    return resultado;
}

public static SqlDataReader EjecutarComandoReader(SqlCommand pComando)
{
    SqlDataReader reader = pComando.ExecuteReader(CommandBehavior.CloseConnection);
    return reader;
}

public static SqlTransaction CrearTransaction()
{
    SqlConnection conexion = ObtenerConexion();
    return conexion.BeginTransaction();
}
```

**Resultado:**
![image](https://github.com/user-attachments/assets/154ca133-ce3c-4143-8e05-5b2a354a973d)

### Configurar String de conexion
Ver video: https://youtu.be/oct6MWqZhoE?si=cSlDKBCiX8MflLDY

## Archivo **ComunDB.cs**
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
// Referencias
using System.Data;
using System.Data.SqlClient;

namespace SistemaElParaisal.DAL
{
    public class ComunDB
    {
        const string strConexion = @"Mi string de conexion aquí";

        private static SqlConnection ObtenerConexion()
        {
            SqlConnection conexion = new SqlConnection(strConexion);
            conexion.Open();
            return conexion;
        }

        public static SqlCommand ObtenerComando()
        {
            SqlCommand comando = new SqlCommand();
            comando.Connection = ObtenerConexion();
            return comando;
        }

        public static SqlCommand ObtenerComando(SqlTransaction pTransaction)
        {
            // Sobre carga del metodo ObtenerComando para crear comandos de transaccion
            SqlCommand comando = new SqlCommand();
            comando.Connection = pTransaction.Connection;
            comando.Transaction = pTransaction;
            return comando;
        }

        public static int EjecutarComando(SqlCommand pComando)
        {
            int resultado = pComando.ExecuteNonQuery();
            pComando.Connection.Close();
            return resultado;
        }

        public static SqlDataReader EjecutarComandoReader(SqlCommand pComando)
        {
            SqlDataReader reader = pComando.ExecuteReader(CommandBehavior.CloseConnection);
            return reader;
        }

        public static SqlTransaction CrearTransaction()
        {
            SqlConnection conexion = ObtenerConexion();
            return conexion.BeginTransaction();
        }
    }
}

```

