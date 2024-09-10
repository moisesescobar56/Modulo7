# TRATAMIENTO DE DATOS - FRONTEND ESCRITORIO

Para mostrar correctamente los datos del Empleado, deberemos configurar el back-end.

![Tratamiento de datos](https://github.com/user-attachments/assets/602283aa-7b03-4b24-b47d-481b8f16271a)

---

### ¿Qué es un ViewModel? [Leer más](https://platzi.com/tutoriales/1395-aspnet-core/4304-model-y-viewmodel-en-mvc-que-son-para-que-se-usan/#:~:text=Un%20viewmodel%20es%20bastante%20similar,de%20datos%20de%20manera%20directa.) 

Un viewmodel es bastante similar a un modelo (clase o entidad), es una clase que se compone de **propiedades o campos**, que usaremos unicamente para hacer **renderizado de vistas**, las instancias de los ViewModels jamas alcanzaran la base de datos de manera directa.

---

Para esta implementacion, se quieren tratar los datos de la **CLASE B** que depende de la **CLASE A**, por ello se creara un **ViewModel** para la clase que presenta los datos en el formulario, la cual es la clase **Empleado**.

![image](https://github.com/user-attachments/assets/43fd7aaf-9b3d-415c-8075-4d5c5895d3b3)


**Explicacion EmpleadoVM:**
- Se creara una nueva clase para el ViewModel (Copia), con las propiedades que se quieren mostrar en la vista: IdEmpleado, Cargo, Nombre, Apellido y Telefono.
- Todas las propiedades seran del mismo tipo que la **clase original**, a excepcion de las dependencias que seran propiedades nuevas que se deseen mostrar.
- **Cargo** sera de tipo **String**, debido a que se desea mostrar **Nombre** en lugar del **IdCargo**.

## PARTE 2 - Programación del ViewModel para cargar Empleados 
**Paso 1:** Ir al Explorador de soluciones y en el proyecto “SistemaElParaisal.UI.WindowsForms”, dar clic derecho y seleccionar **Agregar > Clase**.

![image](https://github.com/user-attachments/assets/47bf37dc-4573-4d32-b055-ce216b9fce8b)

**Paso 2:** Ingresar el nombre **“EmpleadoVM.cs”** y dar clic en **Agregar**. 

![image](https://github.com/user-attachments/assets/ee2a905b-abca-474c-a4a8-5ddf99d545cb)

**Resultado:**

![image](https://github.com/user-attachments/assets/da19666b-76f7-43dd-ae78-ab8f3e0fc583)

**Paso 3:** Seleccionar la clase **“EmpleadoVM.cs”**, luego arrastralar a la carpeta **ViewModels** para mover el archivo. 

![image](https://github.com/user-attachments/assets/4bdaedbb-c085-4556-88ad-9fb623b32dce)
**Mover archivo**
![image](https://github.com/user-attachments/assets/bcfbd941-0ae6-4e90-baac-68583392f6e7)

**Paso 4:** Dar clic en **Aceptar** para mover el archivo de ubicacion.  

![image](https://github.com/user-attachments/assets/6c0b8fc3-6e8b-43a4-9dfb-9561e32286b0)

**Resultado:**
![image](https://github.com/user-attachments/assets/32251871-5116-4ca6-bf80-b05a7307f1a7)

**Paso 5:** Agregar referencias a bibliotecas y establecer la clase como **public** para poder accesar a ella. 
```csharp
// Referencias del proyecto
using SistemaElParaisal.EN;
```
![image](https://github.com/user-attachments/assets/9910ba85-8725-4174-bc0d-ffedef5f1eae)

**Paso 6:** Programar constructor con parámetro de tipo **Empleado** y sus propiedades de la clase. 

```csharp
public EmpleadoVM(Empleado pEmpleado)
{
    // Logica del constructor para cargar propiedades
}

// Propiedades a mostrar, ya ordenadas
public short IdEmpleado { get; set; }
public string Cargo { get; set; } // Mostrar nombre del cargo
public string Nombre { get; set; }
public string Apellido { get; set; }
public string Telefono { get; set; }
```

![image](https://github.com/user-attachments/assets/47fb6021-56aa-4370-8c67-5fae4a59e13f)

- El constructor de EmpleadoVM (Clase copia) recibe un parametro de tipo Empleado (Clase original) para poder cargar posteriormente las propiedades con los datos a mostrar en la interfaz de usuario.

**Paso 7:** Programar la lógica del constructor  para capturar los datos del **Empleado** desde el parámetro **“Empleado pEmpleado”**.

```csharp
// Logica del constructor para cargar propiedades
this.IdEmpleado = pEmpleado.IdEmpleado;
this.Nombre = pEmpleado.Nombre;
this.Apellido = pEmpleado.Apellido;
this.Telefono = pEmpleado.Telefono;

// Cargar propiedad virtual mediante validacion con operador condicional ternario
this.Cargo = (pEmpleado.Cargo != null ? pEmpleado.Cargo.Nombre : "N/A");
```

![image](https://github.com/user-attachments/assets/8c5d0986-0611-482e-9416-571cfdba4583)

##  PARTE 3 - Implementación de tratamiento de datos

**Paso 1:** Desde el Explorador de soluciones, seleccionar y abrir el código del archivo **“AdminEmpleado.cs”**, dar clic derecho sobre el archivo y seleccionar **Ver Código**.

![image](https://github.com/user-attachments/assets/dd634928-df21-4665-b386-30e07d07454e)

**Resultado:**
![image](https://github.com/user-attachments/assets/89af9b17-3c8e-4d4f-93aa-2a3e0c7ff05c)

**Paso 2:** buscar el método **CargarLista()** para modificar su lógica. 

![image](https://github.com/user-attachments/assets/6d5e8370-c6b6-4514-992a-1ec2fcfd3108)

**Paso 3:** modificar la lógica del método **CargarLista()**.

![image](https://github.com/user-attachments/assets/6eeefa20-284f-4923-9f49-1f9ae42abbea)

**Resultado:**
![image](https://github.com/user-attachments/assets/40f1f15a-c897-42ed-8019-8995d993c228)

**Paso 4:** Agregar la nueva lógica del método **CargarLista()** para cargar las propiedades virtuales de la lista. 
```csharp
// Cargar propiedad virtual de Cargo
empleadoBL.CargarCargoVirtual(lista);
// x = es una instancia interna de tipo "Empleado" que se selecciona para convertirlo a un "EmpleadoVM"
listaDataGridView.DataSource = lista.Select( x => new EmpleadoVM(x) ).ToList();
```

![image](https://github.com/user-attachments/assets/7d1222fd-1130-49a0-8ba0-e26ac97dd156)


## PARTE 4 -Ejecutar la aplicacion

**Paso 1:** Iniciar la aplicacion.
![image](https://github.com/user-attachments/assets/c44bad4d-30e8-452e-bc00-594b96ab4a43)

**Paso 2:** Dar clic en el botón **BUSCAR** y se mostrara la lista de empleados guardados.

![image](https://github.com/user-attachments/assets/bfdca3ff-f4bf-4035-8f32-ff78903a2b0f)

**Paso 3:** Detener la aplicacion.
![image](https://github.com/user-attachments/assets/56e319f1-4420-4a3d-9c4f-e72252c891c3)

