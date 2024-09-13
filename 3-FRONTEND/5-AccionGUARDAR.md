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

```
![image](https://github.com/user-attachments/assets/277fd3e8-8ba7-4695-93f8-6fd889765318)

**Resultado:**
![image](https://github.com/user-attachments/assets/a3d5b805-c9e0-4239-b7a8-c2427fdec83e)

**NOTAS:**
- **Exception ex** captura el error en la vista.
- **ModelState.AddModelError()** informa mediante un mensaje los errores al usuario al inicio del formulario.
- **ViewBag.Cargos** se envia a la vista, debido a que la accion **GET** de **Create** muestra una lista de cargos en la vista. Al ser un dato que espera la vista, si no se envia veremo un error siguiente:

![image](https://github.com/user-attachments/assets/f78d014d-c247-4c7c-807c-d453a2e08256)

