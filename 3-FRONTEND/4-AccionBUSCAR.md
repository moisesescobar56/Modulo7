# Programación de funcionalidad BUSCAR

## METODOS DE ENVIO DE DATOS AL SERVIDOR

**GET:** Envia los parametros al controlador mediante la url.

![image](https://github.com/user-attachments/assets/ee9945b4-66be-480e-b714-325cacb86378)

**Usos:** 
- Mostrar vistas, como Inicio de sesion, Menu o Registro.
- Recibir parametros de busqueda y luego mostrar una vista con los datos segun los parametros aplicados.

**POST:** Envia los parametros al controlador mediante un PayLoad. El PayLoad es un objeto que se envia una solicitud como paquete al controlador, tiene una header y un body.
- **Header:** contiene la codificacion (UTF-8), token y direccion de envio de la solicitud.
- **Body:** contiene los datos de la solicitud en formato JSON o XML.
- **JSON (JavaScript Object Notation):** es un formato ligero de intercambio de datos. Leerlo y escribirlo es simple para humanos, mientras que para las máquinas es simple interpretarlo y generarlo. [Leer más](https://developer.mozilla.org/es/docs/Learn/JavaScript/Objects/JSON)

![image](https://github.com/user-attachments/assets/9acf5e9b-2a56-4828-93a5-1b4553a6d5a2)

**Usos**
- Envio de solicitudes POST de registro de datos mediante formularios HTML.

## PARTE 1 - Crear Controlador
**Paso 1:** Dar clic derecho en la carpeta **Controllers** y seleccionar **Agregar > Controlador**.

![image](https://github.com/user-attachments/assets/483c2936-bb18-4a2b-89d3-37b46ee923ef)

**Paso 2:** Seleccionar **Controlador de MVC 5 con acciones de lectura y escritura** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/4e0e53d7-94cb-4565-9610-91b71824faf5)

**Paso 3:** Renombrar en **singular** el controlador con el **nombre de la clase** que se desea crear el **CRUD**. Estructura **Clase + Controller**
```
EmpleadoController
```

![image](https://github.com/user-attachments/assets/fb4578e0-cb41-43dc-948f-1441e4fadafb)

**Paso 4:** Luego de renombrar el controlador, dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/8680273e-f9a8-4082-bf23-a20f9b424807)

**Resultado:**

![image](https://github.com/user-attachments/assets/4a838818-0973-4baa-9d79-cad003d582d3)

**Paso 5:** Eliminar las acciones **GET** de **Details** y **Delete**, 

![image](https://github.com/user-attachments/assets/6335e4a4-74d8-4eec-85dc-7dbafb110a65)

- **GET - Details:** es una vista que permite visualizar los datos completos de un registro mediante su ID.
- **GET - Delete:** es una vista que permite visualizar los datos de un registros mediante su ID y luego confirmar la eliminacion. 

**Resultado:**

![image](https://github.com/user-attachments/assets/c90d63a0-9e16-439f-89d6-08c7fa8b6be0)

## PARTE 2 - Codificar accion Index en Controlador
**Paso 1:** Agregar las **referencias de biblioteca** al controlador.
```csharp
// Referencias
using SistemaElParaisal.EN;
using SistemaElParaisal.BL;
```
![image](https://github.com/user-attachments/assets/48ee0225-c324-47be-bd8f-df22a1237bf3)

**Paso 2:**  Agregar la **conexion a la base de datos** mediante la **EmpleadoBL** de la entidad.
```csharp
//Conexion a la tabla Empleado
EmpleadoBL empleadoBL = new EmpleadoBL();
```
![image](https://github.com/user-attachments/assets/73d507c3-8d16-4043-9011-dbb2dabef10a)

**Paso 3:** Agregar como parametro opcional, recibir un objeto de tipo Empleado en la accion Index.
```csharp
Empleado pEmpleado
```
![image](https://github.com/user-attachments/assets/721a2341-6620-4c3c-9aa8-12a80a34b1a6)

**Paso 4:** Validar que si el parametro es nulo se cree un nuevo objeto vacio.
```csharp
if(pEmpleado == null)
{
    pEmpleado = new Empleado();
}
```
![image](https://github.com/user-attachments/assets/3f35808f-a3b8-474b-a4d8-6ea84b1ba367)

**Paso 5:** Agregar la logica de busqueda en la base de datos y retornar la **List < Empleado >** a la vista. 

**Nota:** Adicionalmente si se tienen los metodos de **CargarClaseVirtual()**, puede incluirse para mostrar las llaves foraneas de la clase.

```csharp
List<Empleado> lista = empleadoBL.Buscar(pEmpleado);

// Cargar propiedades virtuales
empleadoBL.CargarCargoVirtual(lista);

return View(lista);
```

![image](https://github.com/user-attachments/assets/502096a0-c251-47ed-9197-7427794fe2f7)

## PARTE 3 - Crear View Index
**Paso 1:** Seleccionar la accion Index, dar clic derecho y seleccionar **Agregar Vista**

![image](https://github.com/user-attachments/assets/a1897d10-ee88-4ad2-bbba-9fbc4087eefa)

**Paso 2:** Seleccionar **Vista de MVC 5** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/2f4c6b4d-2968-4b35-8b79-1c91e16865d6)

