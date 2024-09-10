
# Programación de funcionalidad ELIMINAR

## PARTE 1 - Programación de Accion ELIMINAR
**Paso 1:** Ir al **Explorador de soluciones** y dar doble clic sobre el archivo **“AdminEmpleadoForm.cs”**. 

![image](https://github.com/user-attachments/assets/2a555a96-267d-481c-856a-91c636083c0c)

**Paso 2:** Dar doble clic sobre el botón **"eliminarButton"**, para generar el **evento click**. 

![image](https://github.com/user-attachments/assets/bf32405d-31f9-4c0d-bfea-685d9b6e3383)

**Resultado:**
![image](https://github.com/user-attachments/assets/a9145938-5210-4f59-beb2-b17034146953)


**Paso 3:** Agregar lógica del botón **ELIMINAR**, en el evento **eliminarButton_Click**. La siguiente lógica es para validar que se haya seleccionado un registro antes de eliminarlo y confirmar si **“¿Desea eliminar el registro seleccionado?”**.
```csharp
// Obtener llave primaria del registro seleccionado
short idTabla = (short)ToolsForms.ObtenerIdGrid(listaDataGridView);
if (idTabla > 0)
{
    if (ToolsForms.MessageBoxConfirmar() == DialogResult.Yes)
    {

    }
}
else
{
    MessageBox.Show("Primero debe seleccionar el registro que desea eliminar", "Info", MessageBoxButtons.OK, MessageBoxIcon.Information);
}
```
![image](https://github.com/user-attachments/assets/8e5004dd-adce-47ff-9474-587543c1074b)

**Paso 4:** Agregar la lógica para eliminar el registro seleccionado. 
```csharp
//Crear instancia del Empleado a eliminar
Empleado empleado = new Empleado() { IdEmpleado = idTabla };
//Ejecutar metodo para eliminar el registro seleccionado
if (empleadoBL.Eliminar(empleado) > 0)
{
    MessageBox.Show("Registro eliminado exitosamente", "Info", MessageBoxButtons.OK, MessageBoxIcon.Information);
    // Actualizar lista simulando click en boton de buscar
    buscarButton.PerformClick();
}
else
{
    MessageBox.Show("Ocurrio un error, por favor intente de nuevo", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
}
```
![image](https://github.com/user-attachments/assets/ef89ba84-d71f-4e21-bf0e-3f52f63bf2f8)

**Paso 5:** Iniciar la aplicacion.
![image](https://github.com/user-attachments/assets/c44bad4d-30e8-452e-bc00-594b96ab4a43)

**Resultado:** Dar clic en el botón **BUSCAR** 
![image](https://github.com/user-attachments/assets/38b5bd74-039e-4d82-b83b-4a7cce0c1773)

**Paso 6:** Seleccionar un registro de la lista de empleado y dar clic en el boton **"ELIMINAR"**
![image](https://github.com/user-attachments/assets/03386867-9917-4c8b-9bfb-bd62447053bc)

**Paso 7:** Seleccionar la opción “SI” en la ventana de confirmacion. 
![image](https://github.com/user-attachments/assets/f9355b05-9d68-4c90-a5b2-20e9959e85f6)

**Resultado:** El registro se ha eliminado. Dar clic en Aceptar.
![image](https://github.com/user-attachments/assets/ef494e75-e893-4112-94ef-5b2474cdb32a)

**Paso 8:** Detener la aplicacion.
![image](https://github.com/user-attachments/assets/56e319f1-4420-4a3d-9c4f-e72252c891c3)
