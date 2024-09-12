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

**Paso 4:** Validar que cuando el parametro es nulo, se cree un nuevo objeto vacio.
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

## PARTE 3 - Crear DropDownList Cargos (Select o ComboBox)

El DropDownList o Select creara una lista desplegable con la que el usuario podra seleccionar facilmente la opcion que desee.

![image](https://github.com/user-attachments/assets/7bc570ed-277b-488c-ab89-47dec8867e69)

**Paso 1:** Ubicarse despues de la accion final Delete del controlador y abrir un espacio.

![image](https://github.com/user-attachments/assets/e8120405-209c-4642-b97c-059c724352bd)

**Paso 2:** Crear un nuevo metodo que retorne una List<SelectListItem> llamado **"DropDownListCargos"**

```csharp
public static List<SelectListItem> DropDownListCargos(byte pId = 0)
{

}
```

![image](https://github.com/user-attachments/assets/832e86a7-4858-447e-9635-c76191d42c31)

- El parametro **pId** debe ser del mismo tipo que la clase del origen de datos que cargara.

**Paso 3:** Agregar la logica para cargar las opciones de Cargo.
```csharp
List<SelectListItem> options = new List<SelectListItem>
{
    new SelectListItem { Value = null, Text = "Seleccionar" }
};

// Buscar registros en la DB
List<Cargo> lista = new CargoBL().Buscar(new Cargo { });

// Agregar opciones
options.AddRange(lista.OrderBy(x => x.Nombre).Select(x => new SelectListItem
{
    Value = x.IdCargo.ToString(), // PK
    Text = x.Nombre,
    Selected = (x.IdCargo == pId),
}).ToList());

return options;
```

![image](https://github.com/user-attachments/assets/54f2b9f4-3190-43bf-85e5-2747696e173f)

**Paso 4:** Agregar el envio del **DropDownListCargos** en la accion **Index** del controlador **Empleado**.
```csharp
// Listas de seleccion filtros
ViewBag.Cargos = DropDownListCargos();
```
![image](https://github.com/user-attachments/assets/54e0216a-12b8-4fd2-b12e-96d491f65aef)

## PARTE 4 - Crear View Index
**Paso 1:** Seleccionar la accion Index, dar clic derecho y seleccionar **Agregar Vista**

![image](https://github.com/user-attachments/assets/a1897d10-ee88-4ad2-bbba-9fbc4087eefa)

**Paso 2:** Seleccionar **Vista de MVC 5** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/2f4c6b4d-2968-4b35-8b79-1c91e16865d6)

**Resultado:**
![image](https://github.com/user-attachments/assets/3ad2f653-80fb-4764-ad85-0d4282fad3d5)

**Paso 3:** Seleccinar en **Plantilla "List"**, en **Clase de modelo** seleccionar la clase **"Empleado (EN)"** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/59870d7c-3676-4106-89cb-5ff50e6619b7)

**Resultado:**
![image](https://github.com/user-attachments/assets/ddaf62e6-077e-4ea9-ac71-9268a5d6c7d6)

**Paso 4:** Iniciar el servidor IIS. 

![image](https://github.com/user-attachments/assets/6eb19f1c-e99c-4215-bd75-479f6fd1307f)

**Paso 5:** Dar clic en la opcion de Empleados del menu.
![image](https://github.com/user-attachments/assets/276482c0-b3c7-4297-9a5b-963c9c04313a)

**Resultado:**
![image](https://github.com/user-attachments/assets/62e2b6e6-be2b-4895-9200-71a5ee04dab2)

**Paso 5:** Detener el servidor IIS.
![image](https://github.com/user-attachments/assets/cd85f0e0-8342-47b9-9426-ac8337b0cc59)

### MEJORAS
🟠 Renombrar el **encabezado**, **agregar campos de filtrado** de los empleados y estilizar el enlace de **Create New**

🔴 Mostrar el **nombre del cargo** del empleado.

🔵 **Eliminar la columnar** de Clave.

🟢 Modificar y **estilizar los botones** de Edit y Delete.

![image](https://github.com/user-attachments/assets/05bd438d-726e-4f16-953a-58153f3cbbde)

## PARTE 5 - Agregar Filtros de Busqueda 

**Paso 1:** Cambiar el ViewBag.Title por "Empleados" y el titulo h2 por "Empleados" y Agregar un <hr> 

![image](https://github.com/user-attachments/assets/ce316f84-9c3c-462d-b209-308197350da9)

**Resultado:**
![image](https://github.com/user-attachments/assets/25684cd1-f4bd-41a4-8b25-70de9bbb6ad0)

**Paso 2:** Eliminar el enlace para **Create New**.

![image](https://github.com/user-attachments/assets/ea5908bd-911e-4b09-8490-0078564bb788)

**Resultado:**
![image](https://github.com/user-attachments/assets/e36a2ec9-b997-447e-a449-652b3ccd2c58)

**Paso 3:** Agregar un **form** para enviar una peticion **GET** a la accion **Index** del controlador **Empleado**.
```razor
<form method="get" action="@Url.Action("Index","Empleado")" class="row">

</form>
```
![image](https://github.com/user-attachments/assets/056b99e4-1889-4ba9-a931-018e0b8ceeb0)

**Paso 4:** Agregar los filtros de Nombre en el HTML.
```razor
<div class="col-12 col-lg-3 mt-2">
    <label class="form-label fw-bold" for="">Nombre o Apellido:</label>
    <input type="text" class="form-control" name="Nombre" placeholder="" />
</div>

<div class="col-12 col-lg-3 mt-2">
    <label class="form-label fw-bold" for="">Cargo:</label>
    @Html.DropDownList("IdCargo", new SelectList(ViewBag.Cargos, "Value", "Text", null), new { @class = "form-select" })
</div>

<div class="col-12 col-lg-2 d-flex align-items-end mt-2">
    <button type="submit" class="btn btn-primary mx-2">BUSCAR</button>
    @Html.ActionLink("NUEVO", "Create", null, new { @Class = "btn btn-secondary" })
</div>
```

![image](https://github.com/user-attachments/assets/5d1bfc28-c6e8-49d7-b472-c89072a05869)

**NOTA:** Los **name** de los input deben coincidir con los nombres de las propiedades de la clase utilizada.

**Paso 5:** Iniciar el servidor IIS. 

![image](https://github.com/user-attachments/assets/6eb19f1c-e99c-4215-bd75-479f6fd1307f)

**Paso 6:** Dar clic en la opcion de Empleados del menu.
![image](https://github.com/user-attachments/assets/276482c0-b3c7-4297-9a5b-963c9c04313a)

**Resultado:**
![image](https://github.com/user-attachments/assets/869a3446-cda3-4c7e-8258-ebe36694c76a)

**Paso 7:** Seleccionar el cargo **Administrador** y da clic en **BUSCAR**.
![image](https://github.com/user-attachments/assets/b6b6db3e-3670-41c1-b844-0a3d91e20bf4)

**Resultado**
![image](https://github.com/user-attachments/assets/beec14f1-08b6-4bce-930e-cd167775d0b0)

**Paso 7:** Detener el servidor IIS.
![image](https://github.com/user-attachments/assets/cd85f0e0-8342-47b9-9426-ac8337b0cc59)

## PARTE 6 - Cargar LLaves Foraneas y Eliminar Columna Clave

**Paso 1:** Ubicarse en la tabla en el **"@foreach"** y modificar el **DisplayFor** de **"IdCargo"**, cambiarlo por **"Cargo.Nombre"**

![image](https://github.com/user-attachments/assets/7e5c1857-9f12-45ad-aa4b-13d5eeb3b2c7)

**Resultado:**
![image](https://github.com/user-attachments/assets/9b93410e-077d-4709-94dc-449a67435a68)

**Paso 2:** Ubicarse en la tabla y eliminar la columna **th Clave** y **td Clave**.

![image](https://github.com/user-attachments/assets/3537a513-35e2-4831-914b-d0ad61934766)

**Resultado:**
![image](https://github.com/user-attachments/assets/668714e8-13d1-4ca6-acb2-388ff671ba54)

## PARTE 7 - Cargar LLaves Foraneas y Eliminar Columna Clave

