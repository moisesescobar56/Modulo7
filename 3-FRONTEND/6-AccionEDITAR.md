# Programación de funcionalidad EDITAR

## PARTE 1 - Codificar Accion Edit
**Paso 0:** Abrir el controlar **EmpleadoController** y verificar:
- Referencias de bibliotectas.
- Conexion a la tabla Empleado en la base de datos mediante Empleado BL.
- DropDownList para cargar la lista de seleccion de Cargos.

![image](https://github.com/user-attachments/assets/eca8fb69-9dcd-4b85-92c9-7c3c9a766f0a)

**NOTA:** Los elemetos solicitados se agregaron al codificar la accion **BUSCAR**.

**Paso 1:** Cambiar el parametro de la accion **GET** de **Edit**, para que reciba un entero de tipo **short**
```csharp
short id
```
![image](https://github.com/user-attachments/assets/29ccd2fb-2381-4cb2-9ca8-a6fadeb79f7e)

**Resultado:**
![image](https://github.com/user-attachments/assets/a6567de8-b250-4fa7-ad09-f147f3887818)


**Paso 2:** Agregar dentro de la accion GET de **Edit**, obtener los datos del empleado por id y un **ViewBag** para enviar la lista de cargos desde el controlador a la vista **Edit**.
```csharp
Empleado empleado = empleadoBL.ObtenerPorId(id);

// Cargar lista de seleccion
ViewBag.Cargos = DropDownListCargos(empleado.IdCargo);

return View(empleado);
```
![image](https://github.com/user-attachments/assets/69b8c1b0-4dbc-47ff-b311-917378a8ca4f)

**Resultado:**
![image](https://github.com/user-attachments/assets/199bfbcc-ba64-4944-9dd9-a627f73cba57)

**Paso 3:** Cambiar el parametro de la accion **POST** de **Edit**, para que reciba un entero de tipo **short** y un objeto de tipo **Empleado**.
```csharp
short id, Empleado pEmpleado
```
![image](https://github.com/user-attachments/assets/4d4dd807-6c00-4d27-baca-7f49235c7e37)

**Resultado:**
![image](https://github.com/user-attachments/assets/ecd879b5-8e5c-4384-bf51-e391a13a2b48)


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
![image](https://github.com/user-attachments/assets/7e236a89-90a4-4a8a-b044-9d37847a9e71)

**NOTAS:**
- **Exception ex** captura el error ocurrido en el controlador.
- **ModelState.AddModelError()** informa mediante un mensaje los errores al usuario al inicio del formulario.
- **ViewBag.Cargos** se envia a la vista, debido a que la accion **GET** de **Create** muestra una lista de cargos en la vista. Al ser un dato que espera la vista, si no se envia veremos un error similar al siguiente:

![image](https://github.com/user-attachments/assets/f78d014d-c247-4c7c-807c-d453a2e08256)

**Paso 5:** Agregar dentro del **Try** de la accion **POST** de Create, la logica para **editar un empleado** si el modelo es valido.
```csharp
// Remover validaciones no obligatorias al editar
ModelState.Remove("Clave");

if (ModelState.IsValid)
{
    int resultado = empleadoBL.Modificar(pEmpleado);

}
```

![image](https://github.com/user-attachments/assets/4af6cebc-cbe8-4f1a-b791-31c910f99f86)

**NOTAS:**
- **ModelState.Remove()** elimina la validacion del modelo de datos.
---
  ### Aclaracion
  En este caso se elimino la validacion, por que el metodo de **Modificar** Empleado en **EmpleadoDAL** no envia el parametro **Clave** al **procedimiento almacenado** en la base de datos.

  ![image](https://github.com/user-attachments/assets/3f6baeb0-d0c7-438f-a1b9-72eec8be01b7)

--- 

**Paso 6:** Agregar la logica para validar el resultado obtenido al modificar al empleado en la base de datos.
```csharp
if (resultado > 0)
{
    TempData["mensaje"] = "Registro actualizado.";
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
![image](https://github.com/user-attachments/assets/14cd29b3-8f3a-4f15-aa02-70dec0785b36)

**NOTAS:**
- **TempData["mensaje"]** crea un espacio de memoria temporal al que se puede accesar desde cualquier parte de la aplicacion.
- **RedirectToAction("Accion", "Controlador")**, se omite el controlador, porque se rediccionara al usuario a la accion Index del mismo controlador.
- **resultado > 0** significa que el registro se actualizo en la base de datos.
- **resultado == -1** significa que el procedimiento almacenado detecto que el registro ya existe.
- **else** cualquier otro error no contemplado mostrara un mensaje generico.

## PARTE 2 - Crear View Edit
**Paso 1:** Seleccionar la accion **GET** de **Edit**, dar clic derecho y seleccionar **Agregar Vista**.

![image](https://github.com/user-attachments/assets/1e31ef5e-c48f-47f0-8d7a-39a3710f0004)

**Paso 2:** Seleccionar **Vista de MVC 5** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/999c3df8-b45a-41f9-b498-fdda01079adb)

**Resultado:**
![image](https://github.com/user-attachments/assets/52f81a26-3b20-4151-856a-1279a23f74f1)


**Paso 3:** Seleccinar en **Plantilla "Edit"**, en **Clase de modelo** seleccionar la clase **"Empleado (EN)"** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/68d76508-ffac-48a0-9ed3-b53097fbd301)

**Resultado:**
![image](https://github.com/user-attachments/assets/085fd423-bbbb-4c5f-ae76-2f22966b8862)


**Paso 4:** Eliminar todo el contenido de la vista **Edit**.

![image](https://github.com/user-attachments/assets/dfdcc577-f6a6-466d-b440-55e64aca25cb)

**Resultado:**
![image](https://github.com/user-attachments/assets/a5354c7a-d365-454b-a5af-1961da05af8f)


**Paso 5:** Abrir la vista **Create** y **copiar** todo el html.

![image](https://github.com/user-attachments/assets/c1b55337-b84c-4204-8ca2-ecdd55b93cd4)

**Resultado:**
![image](https://github.com/user-attachments/assets/7ddd5ba7-a075-415f-ac5a-cea22942e013)


**Paso 6:** **Pegar** todo el HTML de la vista **Create** en la vista **Edit**

![image](https://github.com/user-attachments/assets/a803f142-bf69-46e3-a6d0-3321390d4b65)

**Resultado:**
![image](https://github.com/user-attachments/assets/d3e0464f-5dc6-40e6-8068-7fd31c3db855)

**Paso 7:** Cabiar los titulos de la vista **Edit**.

![image](https://github.com/user-attachments/assets/3d35d56c-5259-493f-87a6-637a99f8993b)

**Resultado:**

![image](https://github.com/user-attachments/assets/65a626af-7452-4baf-9cc6-39d7d8090927)

**Paso 8:** Agregar en un input **HiddenFor** despues del **ValidationSummary** para agregar el **IdEmpleado (PK)** al modelo de datos y que lo reciba el controlador para poder actualizar el registro correcto.
```razor
@Html.HiddenFor(model => model.IdEmpleado)
```
![image](https://github.com/user-attachments/assets/96a4ccfa-c48c-4833-bfc6-775da3ae38e7)

**Resultado:**
![image](https://github.com/user-attachments/assets/2b7dc1c1-f11f-459a-a1d4-49d29d983be0)

**Paso 9:** Cambiar el control **PasswordFor** de la propiedad **Clave** para que este **deshabilitado**, comunicando al usuario que la clave no se modificara.
```razor
@Html.EditorFor(model => model.Clave, new { htmlAttributes = new { @class = "form-control", @disabled = "disabled" } })
```

![image](https://github.com/user-attachments/assets/a81598d0-6cb1-4489-a94b-0becb4573d74)

**Resultado:**
![image](https://github.com/user-attachments/assets/c65b471c-aa6e-49f8-a179-c7277c584c62)


**Paso 10:** Abrir la vista **Index** de Empleado y en la tabla modificar el **@foreach** para habilitar el boton de de **Editar** y **Eliminar**.
```razor
<td class="d-flex d-inline-block justify-content-center">
    <a href="@Url.Action("Edit","Empleado", new { id = item.IdEmpleado })" class="btn btn-warning btn-sm mx-2">
        Editar
    </a>
    <button type="button" class="btn btn-danger btn-sm" >
        Eliminar
    </button>
</td>
```
![image](https://github.com/user-attachments/assets/9273b4d3-f611-465c-874e-618b8915765f)

**Resultado**
![image](https://github.com/user-attachments/assets/74974270-692e-4e58-b17a-dd07b36d7654)

**NOTAS:**
- **Url.Action("Accion", "Controlador", new { parametro = valor })** permite enviar un parametro en la peticion **GET** que se visualizara en la dirección url al cargar la vista de la accion **Edit**.

**Paso 11:** Iniciar el servidor IIS. 

![image](https://github.com/user-attachments/assets/6eb19f1c-e99c-4215-bd75-479f6fd1307f)

**Paso 12:** Dar clic en la opcion de **Empleados** del menu y luego seleccionar un registro y dar clic en **EDITAR**.

![image](https://github.com/user-attachments/assets/e3e59d44-3098-4691-9b36-aa748664e87a)

**Resultado:**
![image](https://github.com/user-attachments/assets/711ad23a-c8b9-49e1-b13c-747661704173)

**Paso 13:** Actualizar los campos con valores diferentes y dar clic en el boton **Guardar**.

![image](https://github.com/user-attachments/assets/77e94015-080a-4550-aada-017f3ca7edd6)

**Resultado:** se debe mostrar una alerta y luego desaparecera automaticamente y se veran los registros.

![image](https://github.com/user-attachments/assets/8cbe4829-dc8a-4f5c-bb0d-19c6c49302f1)

**Listado de empleados actualizado:**

![image](https://github.com/user-attachments/assets/9ab53495-3b9c-45ae-9905-6a44596489c8)

**Paso 15:** Detener el servidor IIS.
![image](https://github.com/user-attachments/assets/cd85f0e0-8342-47b9-9426-ac8337b0cc59)
