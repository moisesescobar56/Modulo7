# Programación de funcionalidad GUARDAR

## PARTE 1 - Codificar Accion Create
**Paso 1:** Abrir el controlar **EmpleadoController** y verificar:
- Referencias de bibliotectas.
- Conexion a la tabla Empleado en la base de datos mediante Empleado BL.
- DropDownList para cargar la lista de seleccion de Cargos.

![image](https://github.com/user-attachments/assets/e2e61219-ea53-416a-bcfd-35c11dc2b84e)

**NOTA:** Los elemetos solicitados se agregaron al codificar la accion BUSCAR.

**Paso 2:** Agregar dentro de la accion GET de **Create** un **ViewBag** para enviar la lista de cargos desde el controlador a la vista **Create**.
```csharp
// Listas de seleccion
ViewBag.Cargos = DropDownListCargos();
```
![image](https://github.com/user-attachments/assets/4b3d202e-cd34-43ec-a4d3-859387ed694d)

**Resultado:**
![image](https://github.com/user-attachments/assets/d79ac73b-3f72-4f43-bc1e-b0b6db35618c)

**Paso 3:** Cambiar el parametro de la accion **POST** de **Create**, para que reciba un objeto de tipo Empleado.
```csharp
Empleado pEmpleado
```
![image](https://github.com/user-attachments/assets/faaf1d9b-99b1-46f1-bc7c-1884d8130d7b)

**Resultado:**
![image](https://github.com/user-attachments/assets/4a12114f-40d2-4b41-9880-cf6472be480e)

**Paso 4:** Actualizar el **Try/Catch** de la accion **POST** de **Create**, para capturar errores y enviarlos a la vista.
```csharp
try
{
    // TODO: Add insert logic here
   
}
catch (Exception ex)
{
    ModelState.AddModelError("", ex.Message);
}
// Cargar lista de seleccion
ViewBag.Cargos = DropDownListCargos(pEmpleado.IdCargo);

return View(pEmpleado);
```
![image](https://github.com/user-attachments/assets/277fd3e8-8ba7-4695-93f8-6fd889765318)

**Resultado:**
![image](https://github.com/user-attachments/assets/a3d5b805-c9e0-4239-b7a8-c2427fdec83e)

**NOTAS:**
- **Exception ex** captura el error ocurrido en el controlador.
- **ModelState.AddModelError()** informa mediante un mensaje los errores al usuario al inicio del formulario.
- **ViewBag.Cargos** se envia a la vista, debido a que la accion **GET** de **Create** muestra una lista de cargos en la vista. Al ser un dato que espera la vista, si no se envia veremos un error similar al siguiente:

![image](https://github.com/user-attachments/assets/f78d014d-c247-4c7c-807c-d453a2e08256)

**Paso 5:** Agregar dentro del **Try** de la accion **POST** de Create, la logica para **guardar un nuevo empleado** si el modelo es valido.
```csharp
if (ModelState.IsValid)
{
    int resultado = empleadoBL.Guardar(pEmpleado);

}
```
![image](https://github.com/user-attachments/assets/984b58ff-1056-4f02-a7c6-40d1535a838f)

**Resultado:**
![image](https://github.com/user-attachments/assets/5fb4ad18-644a-449f-a295-eb6e76a15831)

**Paso 6:** Agregar la logica para validar el resultado obtenido al guardar el nuevo empleado en la base de datos.
```csharp
if (resultado > 0)
{
    TempData["mensaje"] = "Registro guardado.";
    return RedirectToAction("Index");
}
else if (resultado == -1)
{
    ModelState.AddModelError("", "Ya existe un registro con el mismo teléfono.");
}
else
{
    ModelState.AddModelError("", "Error al registrar, intente de nuevo o contacte al soporte.");
}
```
![image](https://github.com/user-attachments/assets/276a6bf6-cd36-454e-86dd-eb99bf441cdf)

**Resultado:**
![image](https://github.com/user-attachments/assets/b33b4784-752c-4639-8968-6049595da4b7)

**NOTAS:**
- **TempData["mensaje"]** crea un espacio de memoria temporal al que se puede accesar desde cualquier parte de la aplicacion.
- **RedirectToAction("Accion", "Controlador")**, se omite el controlador, porque se rediccionara al usuario a la accion Index del mismo controlador.
- **resultado > 0** significa que el registro se inserto en la base de datos.
- **resultado == -1** significa que el procedimiento almacenado detecto que el registro ya existe.
- **else** cualquier otro error no contemplado mostrara un mensaje generico.

## PARTE 2 - Crear View Create
**Paso 1:** Seleccionar la accion **GET** de **Create**, dar clic derecho y seleccionar **Agregar Vista**.

![image](https://github.com/user-attachments/assets/dfb0e4f6-5d8f-4129-a72a-a2ff458f4e7d)

**Paso 2:** Seleccionar **Vista de MVC 5** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/ca0f8826-2b66-46c4-a34b-412464bfcf57)

**Resultado:**
![image](https://github.com/user-attachments/assets/83f4b110-0917-454a-9866-da35ed23a6e2)

**Paso 3:** Seleccinar en **Plantilla "Create"**, en **Clase de modelo** seleccionar la clase **"Empleado (EN)"** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/9eb8e071-77d3-4625-a219-d3f942886025)

**Resultado:**
![image](https://github.com/user-attachments/assets/65bfe940-547f-479b-9b34-7845c0f06bcd)

**Paso 4:** Eliminar el control de la llave primaria (PK) **IdEmpleado**.

![image](https://github.com/user-attachments/assets/e016dc5e-2dd5-4dd0-9c7a-7fa003abc42b)

**Resultado:**
![image](https://github.com/user-attachments/assets/350eff85-110e-46bf-a4b4-caf0a6c78056)

**Paso 5:** Cambiar el control y logica de la propiedade **IdCargo** para mostrar una lista de seleccionar a partir del ViewBag.Cargos.
```razor
@Html.DropDownListFor(model => model.IdCargo, new SelectList(ViewBag.Cargos, "Value", "Text", null), new { @class = "form-select" })
```
![image](https://github.com/user-attachments/assets/145e5dee-fc79-42ab-8456-105301ac5773)

**Resultado:**
![image](https://github.com/user-attachments/assets/78a6c7d5-d6ea-4c07-87f8-a82eb7721295)

**Paso 6:** Cambiar el control de la propiedad **Clave** para mostrar el control como un password.
```razor
@Html.PasswordFor(model => model.Clave, new { @class = "form-control" })
```
![image](https://github.com/user-attachments/assets/27c13884-1254-4adb-b9c7-20c963fb3fd8)

**Resultado:**
![image](https://github.com/user-attachments/assets/9e2e6cea-cfe1-4893-a227-79c655fa20ff)

**Paso 7:** Cabiar los titulos, agregar un <hr/> despues del <h2></h2> y eliminar los fragmentos de html marcados con rojo.

![image](https://github.com/user-attachments/assets/df2bb10a-da76-4452-9f0d-621259ce8cd4)

**Resultado:**
![image](https://github.com/user-attachments/assets/2ec576f0-ed7e-4afc-a64f-432c65ea7642)

**Paso 8:** Agregar en la botonera el **button submit** del formulario, y el **enlace para cancelar** y volver a la vista **Index**.
```razor
<button type="submit" class="btn btn-primary me-2">Guardar</button>
@Html.ActionLink("Cancelar", "Index", "Empleado", new { @class = "btn btn-danger" })
```
![image](https://github.com/user-attachments/assets/7da74e46-0245-482c-b7f1-0cfd6cac7ae5)

**Resultado:**
![image](https://github.com/user-attachments/assets/d65dcd4f-5806-4d09-b55c-a617df2a9120)

**Paso 9:** Abrir la vista **Index** de **Empleado** y al final agregar una **section Scripts** para agregar codigo javascripts.
```razor
@section scripts {
    <script>
        //  codigo js
    </script>

}
```
![image](https://github.com/user-attachments/assets/329cd0de-6733-43a5-96e5-f72aef1db6c7)

**Resultado:**
![image](https://github.com/user-attachments/assets/444fd437-3394-4cf1-82aa-e0a9a65ae631)

**Paso 10:** Agregar en la **section scripts** un **@if** con razor para validar cuando se reciba una variable de memoria temporal **TempData["mensaje"]** diferente de nula, ejecute una alerta de sweetalert.
```razor
@if (TempData["mensaje"] != null)
{
    <script>
        Swal.fire({
            icon: "@(TempData["tipo"]!= null? TempData["tipo"]: "success")",
            title: "@TempData["mensaje"]",
            showConfirmButton: false,
            timer: 1500
        });
    </script>

    TempData["mensaje"] = null; // Vaciar memoria
}
```
![image](https://github.com/user-attachments/assets/918010ac-5b02-4aa5-9f37-4baa66b06ae1)


**Paso 11:** Iniciar el servidor IIS. 

![image](https://github.com/user-attachments/assets/6eb19f1c-e99c-4215-bd75-479f6fd1307f)

**Paso 12:** Dar clic en la opcion de **Empleados** del menu y luego dar clic en **NUEVO**.

![image](https://github.com/user-attachments/assets/9a4f5452-e87a-4c43-97d0-2691e488b6f6)

**Resultado:**
![image](https://github.com/user-attachments/assets/5b2736f8-fe4c-4eac-9d05-beaeba92e4b9)

**Paso 13:** Dar clic en el boton **Guardar**

![image](https://github.com/user-attachments/assets/8dbeefce-e77c-40c9-bb26-d4b2c6d915c4)

**Resultado:**
![image](https://github.com/user-attachments/assets/420f8532-56fc-4617-a8ec-1e0745d436e3)

**Paso 14:** Llenar los campos con los valores correctos y dar clic en el boton **Guardar**.

![image](https://github.com/user-attachments/assets/4f885f14-cf59-4fd6-873a-b5d636fb8430)

**Resultado:** se debe mostrar una alerta y luego desaparecera automaticamente y se veran los registros.
![image](https://github.com/user-attachments/assets/0c7cdbaf-467b-4fb6-84f0-143be211aee4)

**Listado de empleados:**
![image](https://github.com/user-attachments/assets/56466282-cdb5-4047-9732-221ca07af394)

**Paso 15:** Detener el servidor IIS.
![image](https://github.com/user-attachments/assets/cd85f0e0-8342-47b9-9426-ac8337b0cc59)

## PARTE 3 - Mejoras de diseño
**Paso 1:** En la vista Create de Empleado, eliminar todos lis **div** de **apertura y cierre** que tiene la clase **"col-md-10"**.

![image](https://github.com/user-attachments/assets/42f7e587-b769-4149-9066-ed50c5f22da6)

**Resultado:** No olvidar identar el html, se puede hacer automaticamente presionando **Ctrl + A** y **Ctrl + K + F**.

![image](https://github.com/user-attachments/assets/18cb6880-7722-47e1-8f63-12c09412dc28)

**Paso 2:** remplazar la clase de los **LabelFor** por las clases **"form-label fw-bold"**.
```css
form-label fw-bold
```

![image](https://github.com/user-attachments/assets/01a49948-0308-4b4f-8064-0c2be643eb77)

**Resultado:** Presionando **Ctrl + F** podemos desplegar la ventana de busqueda y replamplazar de forma rapida todas las coincidencias.

![image](https://github.com/user-attachments/assets/8484c88f-7e1c-4dfc-b92a-8de6c21411a3)

**Paso 3:** remplazar la clase **"form-horizontal"** por **"row"** y la clase **"form-group"** por las clases "col-lg-8 col-12 mt-2" y "col-lg-4 col-12 mt-2" respectivamente como en la imagen.

![image](https://github.com/user-attachments/assets/075d5b0e-e2c4-4da1-935f-2ab09a283ed5)

**Resultado:** Presionando **Ctrl + F** podemos desplegar la ventana de busqueda y replamplazar de forma rapida todas las coincidencias.

![image](https://github.com/user-attachments/assets/6ae8ff04-5bbb-42a1-8d4b-52225df26f1e)

**Paso 4:** Agregar un salto de fila "row" con la clase "w-100" entre **IdCargo** y **Nombre**, y entre **Apellido** y **Telefono**, por ultimo entre **Clave** y la **Botonera**.
```html
<div class="w-100"></div>
```
![image](https://github.com/user-attachments/assets/67fb0c1c-1334-4508-8611-e61f2325c059)

**Resultado:**
![image](https://github.com/user-attachments/assets/935e3d97-bfab-4721-8034-4afa21163ca9)

**Paso 5:** Iniciar el servidor IIS. 

![image](https://github.com/user-attachments/assets/6eb19f1c-e99c-4215-bd75-479f6fd1307f)

**Paso 6:** Dar clic en la opcion de **Empleados** del menu y luego dar clic en **NUEVO**.
![image](https://github.com/user-attachments/assets/9a4f5452-e87a-4c43-97d0-2691e488b6f6)

**Resultado:**
![image](https://github.com/user-attachments/assets/9fb080cc-52d3-456f-bf51-78c9a3db4bb2)

**Paso 7:** Detener el servidor IIS.
![image](https://github.com/user-attachments/assets/cd85f0e0-8342-47b9-9426-ac8337b0cc59)
