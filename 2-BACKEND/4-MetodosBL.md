# CODIFICACION DE METODOS EN LA CAPA BL

## PARTE 1 - Analisis de metodos segun diagrama de clases
Es recomendable haber definido en algun documento (diagrama de clases) los metodos intermediarios correspondientes a cada clase de nuestro modelo de datos (tablas de la base de datos). Antes de iniciar debemos auxiliarnos en el diagrama de clases e identificar los metodos a crear para cada clase en la capa BL.
![image](https://github.com/user-attachments/assets/b50c27ce-6abc-4d71-bbce-e5aa906673ca)

## PARTE 2 - Creacion de clases y metodos
Los metodos intermediarios con la **"Capa de interfaz de Usuario (UI)"**, deben crearse en la **"Capa de Logica de Negocios (BL)"** como se muestra en el siguiente esquema: 

![image](https://github.com/user-attachments/assets/f87d8242-b7fa-4357-b04f-e44b1ba6549f)

Es importante respetar el orden de creacion del diagrama de clases. Para el ejemplo solo se trabajara con los metodos  de las clases **Cargo y Empleado**, pero en un escenario real se iran creando las clases segun sea necesario.

![image](https://github.com/user-attachments/assets/dcc9bfd9-6e92-4574-aef8-4fd7b8580d91)

**IMPORTANTE:** se recomienda primero crear los archivos de clases, establecerlas como **"public"** y luego codificarlas.

## PARTE 3 - Metodos CargoBL
**Paso 1:** Ubicarse en la capa **"SistemaElParaisal.BL"** y dar clic derecho y seleccionar **"Agregar > Clase"**.

![image](https://github.com/user-attachments/assets/1c265857-26c3-483d-ac8a-e728617f1b10)

**Paso 2:** Nombrar la clase **"CargoBL.cs"** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/0ba0e329-a32a-46e6-8e43-4b5fd5dab681)

**Paso 3:** Establecer como **"public"** la clase **"CargoBL.cs"** y **Guardar** los cambios.

![image](https://github.com/user-attachments/assets/2283fed5-cb8f-4fef-a0c6-04a21bd46a9f)

**Paso 4:** Agregar en la seccion de using las referencias a las bibliotecas del proyecto a utilizar.

```csharp
// Referencias del proyecto
using SistemaElParaisal.EN;
using SistemaElParaisal.DAL;
```
**Resultado:**
![image](https://github.com/user-attachments/assets/33095c68-2fca-46d5-b002-a29092106eac)

**Paso 4:** Codificar los metodos intermediarios de **CargoBL** segun el diagrama de clases y **Guardar** los cambios.

![image](https://github.com/user-attachments/assets/4ee01977-4ec0-4b54-aa1d-5298ad77d3d6)

```csharp
public Cargo ObtenerPorId(byte pIdCargo)
{
    return CargoDAL.ObtenerPorId(pIdCargo);
}
public List<Cargo> Buscar(Cargo pCargo)
{
    return CargoDAL.Buscar(pCargo);
}
```

**Resultado:**
![image](https://github.com/user-attachments/assets/033dbeaa-c627-4c11-8a35-6da5ef22e600)

## PARTE 3 - Metodos EmpleadoBL
**Paso 1:** Ubicarse en la capa **"SistemaElParaisal.BL"** y dar clic derecho y seleccionar **"Agregar > Clase"**.

![image](https://github.com/user-attachments/assets/1c265857-26c3-483d-ac8a-e728617f1b10)

**Paso 2:** Nombrar la clase **"EmpleadoBL.cs"** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/d5c6794b-de84-447f-970e-b7e21532e0b0)

**Paso 3:** Establecer como **"public"** la clase **"EmpleadoBL.cs"** y **Guardar** los cambios.

![image](https://github.com/user-attachments/assets/d82d163f-572f-4be8-af69-7c1c5aa2b4c4)

**Paso 4:** Agregar en la seccion de using las referencias a las bibliotecas del proyecto a utilizar.

```csharp
//Referencias
using System.Security.Cryptography;
// Referencias del proyecto
using SistemaElParaisal.EN;
using SistemaElParaisal.DAL;
```
**Resultado:**
![image](https://github.com/user-attachments/assets/a5b31828-2be9-4d0e-b920-87d4cf8cee93)

**Paso 4:** Codificar los metodos intermediarios de **EmpleadoBL** segun el diagrama de clases y **Guardar** los cambios.

![image](https://github.com/user-attachments/assets/f5f35585-645f-45aa-8347-88327f4dc4ad)

```csharp
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
```
- **CifrarHashSha256:** Metodo secreto creado para cifrar textos en Hash SHA256

**Resultado:**
![image](https://github.com/user-attachments/assets/89f5c14f-a1b8-45e6-8a87-962c7ad3d4cb)

### **NOTA:** Al iniciar un proyecto es recomendable crear primero los archivos con accesibilidad publica y luego codificarlos. 

## Archivo **CargoBL.cs**
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
    public class CargoBL
    {
        public Cargo ObtenerPorId(byte pIdCargo)
        {
            return CargoDAL.ObtenerPorId(pIdCargo);
        }
        public List<Cargo> Buscar(Cargo pCargo)
        {
            return CargoDAL.Buscar(pCargo);
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
    }
}
```
