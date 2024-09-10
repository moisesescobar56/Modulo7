# IMPLEMENTACION DE CLASES EN EL PROYECTO 

## PARTE 1 - Analisis de clases segun modelo de datos
Al inicio de todo proyecto es recomendable haber hecho un analisis de la solucion que quiere construir, con el fin de contemplar que datos llegara a manipular el proyecto al completarlo, de no realizarse el analisis los desarrolladores deberan realizar cambios constantes en el modelo de datos, lo cual afectara la duracion estimada de entrega. 

Para mitigar esta problematica, nos auxilaremos de buenas practicas y modelaremos el diagrama de clases de nuestra solucion. 
![image](https://github.com/user-attachments/assets/b50c27ce-6abc-4d71-bbce-e5aa906673ca)

## PARTE 2 - Creacion de clases
Las clases de nuestro modelo de datos debe crearse en la **"Capa de entidades (EN)"** como se muestra en el siguiente esquema: 
![image](https://github.com/user-attachments/assets/83880c6b-fb27-4909-b10f-94987ce025c5)

Es importante respetar el orden de creacion del diagrama de clases. Para el ejemplo solo se trabajara con las clases **Cargo y Empleado**, pero en un escenario real se deben crear todas las clases.

![image](https://github.com/user-attachments/assets/dcc9bfd9-6e92-4574-aef8-4fd7b8580d91)

**IMPORTANTE:** se recomienda primero crear los archivos de clases, establecerlas como **"public"** y luego codificarlas.

**Paso 1:** Ubicarse en la capa **"SistemaElParaisal.EN"** y dar clic derecho y seleccionar **"Agregar > Clase"**.

![image](https://github.com/user-attachments/assets/1c265857-26c3-483d-ac8a-e728617f1b10)

**Paso 2:** Nombrar la clase **"Cargo.cs"** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/a169492f-7af3-48b6-a0fc-9aa90b28d1c2)

**Paso 3:** Establecer como **"public"** la clase **"Cargo.cs"** y **Guardar** los cambios.

![image](https://github.com/user-attachments/assets/a37431da-d7b2-4e17-81cb-5b19ba0a4ddb)

**Paso 4:** Codificar las propiedades de **Cargo** segun el diagrama de clases y **Guardar** los cambios.

![image](https://github.com/user-attachments/assets/a8c4fc82-99d8-49e6-9bfa-34848c4d4875)

```csharp
public byte IdCargo { get; set; }  
public string Nombre { get; set; }
```

**Resultado:**
![image](https://github.com/user-attachments/assets/dd218686-a067-4863-b735-33619d466793)

**Paso 5:** Ubicarse en la capa **"SistemaElParaisal.EN"** y dar clic derecho y seleccionar **"Agregar > Clase"**.

![image](https://github.com/user-attachments/assets/1c265857-26c3-483d-ac8a-e728617f1b10)

**Paso 6:** Nombrar la clase **"Empleado.cs"** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/05d60d10-1486-4095-ad97-8c7ef732c560)

**Paso 7:** Establecer como **"public"** la clase **"Empleado.cs"** y **Guardar** los cambios.

![image](https://github.com/user-attachments/assets/a7abd1fc-269d-4c32-91ca-ab9634e23bef)

**Paso 8:** Codificar las propiedades de **Empleado** segun el diagrama de clases y **Guardar** los cambios.

![image](https://github.com/user-attachments/assets/c8f86b73-ae8e-42bc-a1f8-a3d33f00cfd8)

```csharp
public short IdEmpleado { get; set; }
public short IdCargo { get; set; } // FK
public string Nombre { get; set; }
public string Apellido { get; set; }
public string Telefono { get; set; }
public string Clave { get; set; }

// Propiedades virtuales para llaves foraneas (FK) para representar la Asociacion
public virtual Cargo Cargo { get; set; }   
```

**Resultado:**
![image](https://github.com/user-attachments/assets/dfc132ed-b795-4909-b922-4b15dca3e41b)

### **NOTA:** Al iniciar un proyecto es recomendable crear primero los archivos con accesibilidad publica y luego codificarlos. 

## Archivo **Cargo.cs**
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace SistemaElParaisal.EN
{
    public class Cargo
    {
        public byte IdCargo { get; set; }
        public string Nombre { get; set; }
    }
}
```

## Archivo **Empleado.cs**
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace SistemaElParaisal.EN
{
    public class Empleado
    {
        public short IdEmpleado { get; set; }
        public byte IdCargo { get; set; } // FK
        public string Nombre { get; set; }
        public string Apellido { get; set; }
        public string Telefono { get; set; }
        public string Clave { get; set; }

        // Propiedades virtuales para llaves foraneas (FK) para representar la Asociacion
        public virtual Cargo Cargo { get; set; }
    }
}
```
