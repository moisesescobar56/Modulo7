# IMPLEMENTACION DE TRATAMIENTO DE DATOS
El tratamiento de datos consiste en presentar la informacion al usuario de una forma comprensible, en la cual se puedan leer los datos y obtener la informacion completa, sin necesidad de tener conocimientos previos de la estructura de los datos. 

![Tratamiento de datos](https://github.com/user-attachments/assets/602283aa-7b03-4b24-b47d-481b8f16271a)

El no tratar los datos, puede llevar a que los usuarios experimenten experiencias de usuario insatisfactorias, aunmentando la propablidad de resistirse al uso del software. 

## PARTE 1 - Analisis de las dependencias

Como punto de partida, debemos analizar nuestro diagrama de clases, mediante el podremos identificar las dependencias de nuestras clases y como se relacionan. 

### **Ejemplo 1:** Dependencia de 2 niveles

![image](https://github.com/user-attachments/assets/ce7d39dd-56f5-46f0-b170-9ee9883c1620)

**Explicación:** 
- La **CLASE A** es independiente, debido a que no tiene **propiedades** que se relacionen a otras clases.
- La **CLASE B** es dependiente, porque tiene propiedades que se relacionan con la **CLASE A**.

Debido a esta dependencia se denomina que es de 2 niveles, en la **CLASE B** se debe agregar la representacion de la dependencia en forma de **propiedad**, se crea una *nueva propiedad "publica"* con el nombre de la **CLASE A** y del tipo de la **CLASE A**, es decir: 
```
+ Cargo: Cargo
```

### **Ejemplo 2:** Dependencia de 3 niveles

![image](https://github.com/user-attachments/assets/efbc6b0a-6061-4fa5-8497-666063c341a2)

**Explicación:** 
- La **CLASE A** es independiente, debido a que no tiene **propiedades** que se relacionen a otras clases.
- La **CLASE B** es dependiente, porque tiene propiedades que se relacionan con la **CLASE A**.
- La **CLASE C** es dependiente, porque tiene propiedades que se relacionan con la **CLASE B** y la **CLASE B** a su vez tiene una dependencia de la **CLASE A**.

Debido a esta dependencia se denomina que es de 3 niveles, en la **CLASE B** se debe agregar la representacion de la dependencia en forma de **propiedad**, se crea una *nueva propiedad "publica"* con el nombre de la **CLASE A** y del tipo de la **CLASE A**, es decir: 
```
+ Cargo: Cargo
```

Y en la **CLASE C** se debe agregar la representacion de la dependencia en forma de **propiedad**, se crea una *nueva propiedad "publica"* con el nombre de la **CLASE B** y del tipo de la **CLASE B**, es decir: 
```
+ Empleado: Empleado
```

## PARTE 2 - Configuracion de las clases

Para realizar este ejemplo **estrictamente** debemos tener agregada la propiedad virtual de la **dependencia**, en este caso la dependencia de ***IdCargo***, por lo cual es obligatorio haber  programado antes la clase Cargo con sus Propiedades en la capa “SistemaElParaisal.EN” y que  ambas clases sean públicas ***(public class)***.

![image](https://github.com/user-attachments/assets/7e63e2bf-7d71-4b5e-b1aa-e2a8f4a69214)

# CODIGO FUENTE

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


