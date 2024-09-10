# Programación de funcionalidad EDITAR

## PARTE 1 - Programación de Accion EDITAR
**Paso 1:** Ir al **Explorador de soluciones** y dar doble clic sobre el archivo **“AdminEmpleadoForm.cs”**. 

![image](https://github.com/user-attachments/assets/2a555a96-267d-481c-856a-91c636083c0c)

**Paso 2:** Dar doble clic sobre el botón **"editarButton"**, para generar el **evento click**. 

![image](https://github.com/user-attachments/assets/a142cd27-2438-4586-9f42-12d206901b37)

**Resultado:**
![image](https://github.com/user-attachments/assets/7cd35332-c620-4091-b50f-2e21b45a3c7a)

**Paso 3:** Agregar lógica del botón **EDITAR**, en el evento **editarButton_Click**.
```csharp
// Obtener llave primaria (PK) del registro seleccionado
short idTabla = (short)ToolsForms.ObtenerIdGrid(listaDataGridView);
if (idTabla > 0)
{
    // Abrir formulario para modificar el empleado seleccionado
    RegistroEmpleadoForm formEditar = new RegistroEmpleadoForm();
    formEditar.idEmpleado = idTabla;
    formEditar.ShowDialog();

    // Actualizar lista simulando click en boton de BUSCAR
    buscarButton.PerformClick();
}
else
{
    MessageBox.Show("Primero debe seleccionar el registro que desea editar", "Info", MessageBoxButtons.OK, MessageBoxIcon.Information);
}
```
![image](https://github.com/user-attachments/assets/d6cb8ab6-a714-4a76-a25f-d8924c8044af)


**Paso 4:** Iniciar la aplicacion.
![image](https://github.com/user-attachments/assets/c44bad4d-30e8-452e-bc00-594b96ab4a43)

**Resultado:** Dar clic en el botón **BUSCAR** 
![image](https://github.com/user-attachments/assets/39746863-9010-496e-a215-52e37a82a5aa)

**Paso 5:** Seleccionar un registro de la lista de empleado y dar clic en el boton **"EDITAR"**
![image](https://github.com/user-attachments/assets/5a98216d-c0bb-430f-addc-5a311b93bc66)

**Resultado:**
![image](https://github.com/user-attachments/assets/ce6d3c70-ebec-4029-bcd2-50e800a8424f)

**Paso 6:** Cambiar el nombre de “Luis Felipe” a “Luis Fernando” y el cargo de "Vendedor" a "Administrador", luego dar clic en **"GUARDAR"**.
![image](https://github.com/user-attachments/assets/f082792b-81df-44e6-bca4-a757e6f8b77e)

**Paso 7:** Dar clic en **Aceptar**.
![image](https://github.com/user-attachments/assets/845e2b34-845e-4849-a98e-5c75e331e304)

**Resultado:** El registro se ha actualizado.

![image](https://github.com/user-attachments/assets/3bcd2b2f-0bad-4f82-a513-39c2f33424ad)

**Paso 9:** Detener la aplicacion.
![image](https://github.com/user-attachments/assets/56e319f1-4420-4a3d-9c4f-e72252c891c3)


## CORRECCIONES
En el procedimiento almacenado **"SP_ModificarEmpleado"** se actualizo, debido a que al momento de editar los registros, no se actualizara el campo ***"Clave"***, por lo cual solo se modifico para que se actualice con el mismo valor.

![image](https://github.com/user-attachments/assets/15f55e0b-6f1a-4169-aff8-bc728a2add00)

```sql
USE ElParaisalDB
GO
-- Modificar Empleado
ALTER PROCEDURE SP_ModificarEmpleado
    @IdEmpleado SMALLINT,
    @IdCargo TINYINT,
    @Nombre VARCHAR(50),
    @Apellido VARCHAR(50),
    @Telefono NCHAR(8),
    @Clave VARCHAR(250)
AS
BEGIN
    IF EXISTS (SELECT * FROM Empleado WHERE Telefono = @Telefono AND IdEmpleado != @IdEmpleado)
		BEGIN
			PRINT 'Empleado ya registrado';
		END
    ELSE
		BEGIN
			UPDATE Empleado
			SET IdCargo = @IdCargo, Nombre = @Nombre, Apellido = @Apellido, Telefono = @Telefono, Clave = Clave
			WHERE IdEmpleado = @IdEmpleado;
			PRINT 'Empleado modificado correctamente';
		END
END;
GO
```
