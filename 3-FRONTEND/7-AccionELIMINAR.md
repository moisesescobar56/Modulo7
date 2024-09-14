# Programación de funcionalidad ELIMINAR

## PARTE 1 - Codificar Accion Delete
**Paso 0:** Abrir el controlar **EmpleadoController** y verificar:
- Referencias de bibliotectas.
- Conexion a la tabla Empleado en la base de datos mediante Empleado BL.

![image](https://github.com/user-attachments/assets/731c54fa-0ee6-444f-821b-b8c6b6cea90e)

**NOTA:** Los elemetos solicitados se agregaron al codificar la accion **BUSCAR**.

**Paso 1:** Cambiar el parametro de la accion **POST** de **Delete**, para que reciba solo un entero de tipo **short**
```csharp
short id
```
![image](https://github.com/user-attachments/assets/f06a0992-0139-4e2f-88f6-76eaf1823e41)

**Resultado:**
![image](https://github.com/user-attachments/assets/89571018-f1c0-4c09-a9ee-c0a28aa7ab48)

**Paso 2:** Actualizar el **Try/Catch** de la accion **POST** de **Create**, para capturar errores y enviarlos a la vista.
```csharp
try
{
    // TODO: Add delete logic here

}
catch (Exception ex)
{
    TempData["tipo"] = "error";
    TempData["mensaje"] = ex.Message;
}
return RedirectToAction("Index");
```
![image](https://github.com/user-attachments/assets/0e411714-fb9a-4e83-a24a-ca08762f76cf)

**NOTAS:**
- **Exception ex** captura el error ocurrido en el controlador.
- **TempData["mensaje"]** y **TempData["tipo"]** crea un espacio de memoria temporal al que se puede accesar desde cualquier parte de la aplicacion.
- **RedirectToAction("Accion", "Controlador")**, se omite el controlador, porque se rediccionara al usuario a la accion Index del mismo controlador.

**Paso 5:** Agregar dentro del **Try** de la accion **POST** de **Delete**, la logica para **eliminar un empleado** y validar el resultado obtenido al eliminar al empleado en la base de datos.
```csharp
int resultado = empleadoBL.Eliminar(new Empleado { IdEmpleado = id });

if (resultado > 0)
{
    TempData["mensaje"] = "Registro eliminado.";
}
else
{
    TempData["tipo"] = "error";
    TempData["mensaje"] = "Error al eliminar, intente de nuevo o contacte al soporte.";
}
```

![image](https://github.com/user-attachments/assets/891928b0-fb81-4876-8f7c-123cb447b683)

**NOTAS:**
- **TempData["mensaje"]** y **TempData["tipo"]** crea un espacio de memoria temporal al que se puede accesar desde cualquier parte de la aplicacion.
- **resultado > 0** significa que el registro se elimino de la base de datos.
- **else** cualquier otro error no contemplado mostrara un mensaje generico.

---
## PARTE 2 - Utilidades.js
Si accedemos al archivo **utilidades.js** que se encuentra en la carpeta de **Scritpts**, encontraremos que dentro tiene una funcion recibe como **parametro una url** y ejecuta una alerta de [**Sweet Alert 2**](https://sweetalert2.github.io/) que mostrara un cuadro de dialogo (Ventana de confirmacion de SI o NO).
 
![image](https://github.com/user-attachments/assets/cc2282d8-88c6-4ac6-882b-d692bde3ae3d)

#### Mediante esta function enviaremos la solicitud para eliminar el registro seleccionado por el usuario.

---
## PARTE 3 - Configurar el button Eliminar
**Paso 1:** Abrir la vista **Index** del controlador **Empleado**. 

![image](https://github.com/user-attachments/assets/30857945-fa85-4b5b-982f-c4d5c56a533d)

**Paso 2:** Ubicarse dentro de la tabla en el **@foreach** y buscar la **seccion de botones** el boton **Eliminar**. 

![image](https://github.com/user-attachments/assets/aa5d402b-34d8-4fe4-815c-bd2fc3e718ae)

**Paso 3:** Agregar el evento html **onclick** y ejecutar la funcion **funcEliminar(url)** pasandole la url para enviar mediante **POST** la solicitud de eliminar a la accion **Delete**.
```html
<button type="button" class="btn btn-danger btn-sm"
        onclick="funcEliminar('@Url.Action("Delete", "Empleado", new { id = item.IdEmpleado })')">
    Eliminar
</button>
```

![image](https://github.com/user-attachments/assets/602148d6-9e8c-4659-b59b-647b4e9ac7a4)

**NOTAS:**
- **Url.Action("Accion", "Controlador", new { parametro = valor })** permite crear una url que incluya un parametros. Se puede enviar mas de un parametro si la accion los espera.

**Paso 4:** Iniciar el servidor IIS. 

![image](https://github.com/user-attachments/assets/6eb19f1c-e99c-4215-bd75-479f6fd1307f)

**Paso 5:** Dar clic en la opcion de **Empleados** del menu y luego seleccionar un registro y dar clic en **ELIMINAR**.

![image](https://github.com/user-attachments/assets/05fac6c9-c1c3-454e-8573-1f0e010d6868)

**Resultado:**
![image](https://github.com/user-attachments/assets/8267c0d3-8f20-4b56-9f64-6779deee258e)

**Paso 6:** Leer el mensaje de la ventana de dialogo y dar clic en **Confirmar**.

![image](https://github.com/user-attachments/assets/7a0e7cf0-5288-4e0a-932d-2bfeac6d0bd2)

**Resultado:** se debe mostrar una alerta y luego desaparecera automaticamente y se veran los registros.

![image](https://github.com/user-attachments/assets/7f319300-00d7-408a-8952-5c803e48da87)

**Listado de empleados actualizada:**

![image](https://github.com/user-attachments/assets/1c4160f0-da0a-4e01-affc-0df3f316afbf)

**Paso 7:** Detener el servidor IIS.
![image](https://github.com/user-attachments/assets/cd85f0e0-8342-47b9-9426-ac8337b0cc59)
