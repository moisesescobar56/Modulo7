# Programación de funcionalidad GUARDAR

## PARTE 1 - Programación del boton NUEVO
**Paso 1:** Abrir el formulario "AdminEmpleadoForm.cs".

![image](https://github.com/user-attachments/assets/214b7ef7-e17a-4143-92e9-27898ad64d7c)

**Paso 2:** dar doble clic sobre el boton **"NUEVO"** para generar el “Evento Click”

![image](https://github.com/user-attachments/assets/0cf8db99-dc62-4e72-b7ba-70b297d06b6f)

**Resultado:**
![image](https://github.com/user-attachments/assets/002be4e1-f887-4b23-ae23-ad7f1dfce22e)

**Paso 3:** Programar la lógica para abrir el formulario **“RegistroEmpleadoForm.cs”**
```csharp
// Abrir formulario para registrar un nuevo cliente 
RegistroEmpleadoForm frmResigistro = new RegistroEmpleadoForm();
frmResigistro.ShowDialog();
```

![image](https://github.com/user-attachments/assets/a9263d86-be4e-4c72-a731-2e2cf196add2)

## PARTE 2 - Codificacion de ComboBox y Load
**Paso 1:** Abrir el formulario **"RegistroEmpleadoForm.cs"**.

![image](https://github.com/user-attachments/assets/db804307-c140-44fc-afd9-2ca0712604f7)

**Paso 2:** Agregar un ErrorProvider al formulario, arrastrándolo y dejándolo caer en el Diseño del formulario “RegistroEmpleadoForm.cs” 

![image](https://github.com/user-attachments/assets/79ce254b-03f7-490e-bf61-dd5e01e723ff)

**Paso 3:** Cambiar el nombre del “errorProvider1” a **“errorProvider”** en la ventana de Propiedades.

![image](https://github.com/user-attachments/assets/acd75045-034d-4af6-b8ee-d87a978f41cb)

**Paso 4:** Seleccionar el formulario “RegistroEmpleadoForm.cs” en el diseño y en la ventana de propiedades, seleccionar la opción **Eventos**.

![image](https://github.com/user-attachments/assets/663c9dbe-9ebd-4a73-8f0f-35c097bbaa32)

**Paso 5:** Generar el evento Load del formulario, dando doble click en la opción Load. 

![image](https://github.com/user-attachments/assets/5b1e5000-6bf6-4023-8f55-8c5f169b459b)

**Resultado**
![image](https://github.com/user-attachments/assets/b36f2d19-abb9-41e9-a386-0c1b8a515d1f)

**Paso 6:** Agregar referencias a bibliotecas en el formulario.
```csharp
//Referencias del proyecto
using SistemaElParaisal.EN;
using SistemaElParaisal.BL;
// Extension de metodos
using ToolsForms;
```
![image](https://github.com/user-attachments/assets/1595b46b-9e7b-4790-8aff-be567cf53dc4)

**Paso 7:** Agregar conexión a la base de datos mediante instancia a la clase **EmpleadoBL**. 
```csharp
// Conexion a la tabla de Empleados en la DB
EmpleadoBL empleadoBL = new EmpleadoBL();
```
![image](https://github.com/user-attachments/assets/4081210e-5c66-4f29-8c64-206696a29d35)

**Paso 8:** Agregar a la lógica la variable “idEmpleado” para almacenar la llave primaria de cualquier registro de "Empleado" que se desee modificar. Debe **asignar el mismo tipo de dato** a como en esta en la **EN**.
```csharp
// Variables
public short idEmpleado = 0; // variable del mismo tipo que la PK
```
![image](https://github.com/user-attachments/assets/165cb33b-8e79-4c64-97e4-a0811c69fd2a)

**Paso 9:** Programar la lógica del método interno **"CargarCargos()"** que nos permitirá mostrar en forma de lista de selección las cargos en un formulario.
```csharp
public void CargarCargos()
{
    // Conexion a la tabla de Cargo en la DB
    CargoBL cargoBL = new CargoBL();

    // Inicializar lista 
    List<Cargo> cargos = new List<Cargo>();
    cargos.Add(new Cargo { IdCargo = 0, Nombre = "SELECCIONAR" });

    // Obtener lista de Cargos de la DB
    cargos.AddRange(cargoBL.Buscar(new Cargo()));
    cargoComboBox.DataSource = cargos;

    // Configurar texto y valor de la lista de seleccion
    cargoComboBox.DisplayMember = "Nombre";
    cargoComboBox.ValueMember = "IdCargo";
}
```

![image](https://github.com/user-attachments/assets/11edff1d-8ed1-4586-8887-92c935525fc5)

**Paso 10:** Codificar la lógica del evento Load.
```csharp
//Cargar ComboBoxs en el formulario
CargarCargos();

if (idEmpleado > 0)
{
    // Si "idEmpleado" es mayor que cero se esta modificando un empleado
    Empleado empleado = empleadoBL.ObtenerPorId(idEmpleado); // buscar por id al empleado
    
    // Cargar datos del empleado en los controles del formulario
    nombreTextBox.Text = empleado.Nombre;
    apellidoTextBox.Text = empleado.Apellido;
    telefonoTextBox.Text = empleado.Telefono;
    cargoComboBox.SelectedValue = empleado.IdCargo; // seleccionar cargo segun empleado
    claveTextBox.Enabled = false; // deshabilidad TextBox de clave

    this.Text = "Editar Empleado"; // titulo del formulario
}
```
![image](https://github.com/user-attachments/assets/b161703f-1ad3-4046-b561-34a9f3df20dc)

**Paso 11:** Iniciar la aplicacion.
![image](https://github.com/user-attachments/assets/c44bad4d-30e8-452e-bc00-594b96ab4a43)

**Paso 12:** Dar clic en **NUEVO**.
![image](https://github.com/user-attachments/assets/4f0e5fd4-9e7d-453d-9bfa-ad798d9f825f)

**Resultado:** Verificar que se cargue la lista de desplegable **Cargos**.
![image](https://github.com/user-attachments/assets/d4358df2-a637-4a4a-ad1a-9faf9eb50809)

**Paso 12:** Detener la aplicacion.
![image](https://github.com/user-attachments/assets/56e319f1-4420-4a3d-9c4f-e72252c891c3)

## PARTE 3 - Programación de Accion GUARDAR y CANCELAR

**Paso 1:** Dar doble clic sobre el botón **"cancelarButton"**, para generar el evento **click**. 

![image](https://github.com/user-attachments/assets/c02aa937-b678-4c9e-80ba-e10f42133b1c)

**Resultado:**
![image](https://github.com/user-attachments/assets/deb6c81f-aece-46f5-9736-d29e5cbd30bf)

**Paso 2:** Codificar la lógica del botón **CANCELAR**. 
```csharp
// this = Formulario actual y Close() = evento para cerrar un formulario
this.Close();
```
![image](https://github.com/user-attachments/assets/c21d1362-8c1b-4efb-8cb2-5baf7c2ce755)

**Paso 3:** Dar doble clic sobre el botón **"guardarButton"**, para generar el evento click.

![image](https://github.com/user-attachments/assets/b07387e1-6a3a-4cad-9455-9749bac82741)

**Resultado:**
![image](https://github.com/user-attachments/assets/245d9d6d-b338-45b9-bdc1-757639daf51c)

**Paso 4:** Programar método interno **"ValidarControles"** para validar que el usuario complete los campos obligatorios del formulario. 
```csharp
private bool ValidarControles()
{
    errorProvider.Clear(); // Limpiar errores
    bool datosValidos = true; // Si el valor es True los datos son correctos
    datosValidos = errorProvider.ValidarControl(nombreTextBox, "Campo obligatorio");
    datosValidos = errorProvider.ValidarControl(apellidoTextBox, "Campo obligatorio");
    datosValidos = errorProvider.ValidarControl(telefonoTextBox, "Campo obligatorio", @"^[0-9]{8}$");
    datosValidos = errorProvider.ValidarControl(cargoComboBox, "Campo obligatorio");
    
    if (idEmpleado == 0) // id es cero, no se esta editando
    {
        // Agregar aqui validaciones necesarias al guardar un nuevo registro
        datosValidos = errorProvider.ValidarControl(claveTextBox, "Campo obligatorio", @"^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[A-Za-z\d]{8,}$");
    }
return datosValidos;
}
```

![image](https://github.com/user-attachments/assets/8ea99a76-7f24-4f1f-9749-dabd96e1a8b2)

### IMPLEMENTACION DE EXTENSION DE ERROR PROVIDER


Este metodo recibe 2 parametros obligatorios (**pControl**, **pMensaje**) y 1 opcional ([**pRegex** = null]).
1. **Control pControl:** puede ser cualquier control de tipo **TextBox** o **ComboBox**.
2. **string pMensaje:** define el mensaje a mostrar en el control si el valor del control no es valido.
3. **string pRegex:** define la [Regular Expression](https://www.ibm.com/docs/es/i/7.5?topic=expressions-regular), funciona comparando el valor del control versus el REGEX(Regular Expression), si coincide el dato es valido.

![image](https://github.com/user-attachments/assets/b3c3a4f4-4b78-4651-846d-37ea3b497e09)

**NOTA:** REGEX es util para validar formatos como numero de telefono, dui o ID, fechas, reglas para contraseñas, etc.
- Regex que valida numero de telefono de 8 digitos: ^[0-9]{8}$
- Regex que valida contraseña que contengas mayusculas, minusculas y numeros: ^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[A-Za-z\d]{8,}$
- Los Regex se pueden solicitar a ***ChatGPT, Copilot o cualquier IA***.

## CONTINUACION 
**Paso 5:** Codificar la lógica del botón **GUARDAR**. Agregar un **[Try/Catch](https://learn.microsoft.com/es-es/dotnet/csharp/fundamentals/exceptions/exception-handling)** para gestionar los errores de ejecución al guardar un registro. 
```csharp
int resultado = 0; // resultado del comando en la DB
try
{

}
catch (Exception ex)
{
    MessageBox.Show(ex.Message, "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
}
```
![image](https://github.com/user-attachments/assets/160106fb-107d-40cc-805d-a211af3aaad7)

**Paso 6:** Agregar a la lógica un if para la validación de campos obligatorios. 
```csharp
if (ValidarControles() == true)
{

}
```
![image](https://github.com/user-attachments/assets/dc9b85dc-aa56-459a-b6bd-b8d306e7e697)

**Paso 7:** Agregar lógica para obtener los valores en los controles del formulario.
```csharp
// Capturar datos del formulario
Empleado empleado = new Empleado(); // instancia de empleado
empleado.Nombre = nombreTextBox.Text;
empleado.Apellido = apellidoTextBox.Text;
empleado.Telefono = telefonoTextBox.Text;
empleado.Clave = claveTextBox.Text;
empleado.IdCargo = (byte)cargoComboBox.SelectedValue;
```
![image](https://github.com/user-attachments/assets/9580ba1d-9412-4a61-8253-dfb2e694aba2)

**IMPORTANTE:** la conversion al obtener un valor del comboBox debe ser segun el tipo de dato de la propiedad en la clase.

**Paso 8:** Agregar lógica para GUARDAR o MODIFICAR un registro de empleado, verificando si se registro o no.
```csharp
if (idEmpleado == 0)
{                        
    resultado = empleadoBL.Guardar(empleado); //Se esta guardando un nuevo empleado
}
else
{                        
    empleado.IdEmpleado = idEmpleado; 
    resultado = empleadoBL.Modificar(empleado); //Se esta modificando un empleado existente
}
// Verficiacion del resultado en la base de datos
if (resultado > 0)
{
    MessageBox.Show("Registro guardado exitosamente", "Info", MessageBoxButtons.OK, MessageBoxIcon.Information);
    this.Close();
}
else
{
    MessageBox.Show("Ocurrio un error, por favor intentelo de nuevo", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
}
```

![image](https://github.com/user-attachments/assets/ade3a058-4893-47dc-b4cf-f874f0f84ab7)

**Metodo completo:**
```csharp
int resultado = 0; // resultado del comando en la DB
try
{
    if (ValidarControles() == true)
    {
        // Capturar datos del formulario
        Empleado empleado = new Empleado(); // instancia de empleado
        empleado.Nombre = nombreTextBox.Text;
        empleado.Apellido = apellidoTextBox.Text;
        empleado.Telefono = telefonoTextBox.Text;
        empleado.Clave = claveTextBox.Text;
        empleado.IdCargo = (byte)cargoComboBox.SelectedValue;

        if (idEmpleado == 0)
        {                        
            resultado = empleadoBL.Guardar(empleado); //Se esta guardando un nuevo empleado
        }
        else
        {                        
            empleado.IdEmpleado = idEmpleado; 
            resultado = empleadoBL.Modificar(empleado); //Se esta modificando un empleado existente
        }
        // Verficiacion del resultado en la base de datos
        if (resultado > 0)
        {
            MessageBox.Show("Registro guardado exitosamente", "Info", MessageBoxButtons.OK, MessageBoxIcon.Information);
            this.Close();
        }
        else
        {
            MessageBox.Show("Ocurrio un error, por favor intentelo de nuevo", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
        }
    }
}
catch (Exception ex)
{
    MessageBox.Show(ex.Message, "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
}
```

**Paso 9:** Iniciar la aplicacion.
![image](https://github.com/user-attachments/assets/c44bad4d-30e8-452e-bc00-594b96ab4a43)

**Paso 10:** Dar clic en **NUEVO**.
![image](https://github.com/user-attachments/assets/4f0e5fd4-9e7d-453d-9bfa-ad798d9f825f)

**Paso 11:** Completar los datos del formulario y dar clic en el boton **"GUARDAR"**
![image](https://github.com/user-attachments/assets/2b8db17e-ee89-452f-9eac-ac964d44197e)

**Paso 12:** Dar clic en **Aceptar** y luego dar clic en el boton **BUSCAR**.
![image](https://github.com/user-attachments/assets/53ab1e6a-ae22-44b6-9ce4-caa06904864e)

**Resultado:**
![image](https://github.com/user-attachments/assets/e8adeb29-375e-4635-a378-16929441eee8)

**Paso 13:** Detener la aplicacion.
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

## Archivo **RegistroEmpleadoForm.cs**
```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
//Referencias del proyecto
using SistemaElParaisal.EN;
using SistemaElParaisal.BL;
// Extension de metodos
using ToolsForms;

namespace SistemaElParaisal.UI.WinForms
{
    public partial class RegistroEmpleadoForm : Form
    {
        // Conexion a la tabla de Empleados en la DB
        EmpleadoBL empleadoBL = new EmpleadoBL();

        // Variables
        public short idEmpleado = 0; // variable del mismo tipo que la PK

        public RegistroEmpleadoForm()
        {
            InitializeComponent();
        }

        public void CargarCargos()
        {
            // Conexion a la tabla de Cargo en la DB
            CargoBL categoriaBL = new CargoBL();

            // Inicializar lista 
            List<Cargo> cargos = new List<Cargo>();
            cargos.Add(new Cargo { IdCargo = 0, Nombre = "SELECCIONAR" });

            // Obtener lista de Cargos de la DB
            cargos.AddRange(categoriaBL.Buscar(new Cargo()));
            cargoComboBox.DataSource = cargos;

            // Configurar texto y valor de la lista de seleccion
            cargoComboBox.DisplayMember = "Nombre";
            cargoComboBox.ValueMember = "IdCargo";
        }

        private void RegistroEmpleadoForm_Load(object sender, EventArgs e)
        {
            //Cargar ComboBoxs en el formulario
            CargarCargos();

            if (idEmpleado > 0)
            {
                // Si "idEmpleado" es mayor que cero se esta modificando un empleado
                Empleado empleado = empleadoBL.ObtenerPorId(idEmpleado); // buscar por id al empleado

                // Cargar datos del empleado en los controles del formulario
                nombreTextBox.Text = empleado.Nombre;
                apellidoTextBox.Text = empleado.Apellido;
                telefonoTextBox.Text = empleado.Telefono;
                cargoComboBox.SelectedValue = empleado.IdCargo; // seleccionar cargo segun empleado
                claveTextBox.Enabled = false; // deshabilidad TextBox de clave

                this.Text = "Editar Empleado"; // titulo del formulario
            }
        }

        private void cancelarButton_Click(object sender, EventArgs e)
        {
            // this = Formulario actual y Close() = evento para cerrar un formulario
            this.Close();
        }

        private bool ValidarControles()
        {
            errorProvider.Clear(); // Limpiar errores
            bool datosValidos = true; // Si el valor es True los datos son correctos
            datosValidos = errorProvider.ValidarControl(nombreTextBox, "Campo obligatorio");
            datosValidos = errorProvider.ValidarControl(apellidoTextBox, "Campo obligatorio");
            datosValidos = errorProvider.ValidarControl(telefonoTextBox, "Campo obligatorio", @"^[0-9]{8}$");
            datosValidos = errorProvider.ValidarControl(cargoComboBox, "Campo obligatorio");

            if (idEmpleado == 0) // id es cero, no se esta editando
            {
                // Agregar aqui validaciones necesarias al guardar un nuevo registro
                datosValidos = errorProvider.ValidarControl(claveTextBox, "Campo obligatorio", @"^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[A-Za-z\d]{8,}$");
            }
            return datosValidos;
        }

        private void guardarButton_Click(object sender, EventArgs e)
        {
            int resultado = 0; // resultado del comando en la DB
            try
            {
                if (ValidarControles() == true)
                {
                    // Capturar datos del formulario
                    Empleado empleado = new Empleado(); // instancia de empleado
                    empleado.Nombre = nombreTextBox.Text;
                    empleado.Apellido = apellidoTextBox.Text;
                    empleado.Telefono = telefonoTextBox.Text;
                    empleado.Clave = claveTextBox.Text;
                    empleado.IdCargo = (byte)cargoComboBox.SelectedValue;

                    if (idEmpleado == 0)
                    {                        
                        resultado = empleadoBL.Guardar(empleado); //Se esta guardando un nuevo empleado
                    }
                    else
                    {                        
                        empleado.IdEmpleado = idEmpleado; 
                        resultado = empleadoBL.Modificar(empleado); //Se esta modificando un empleado existente
                    }
                    // Verficiacion del resultado en la base de datos
                    if (resultado > 0)
                    {
                        MessageBox.Show("Registro guardado exitosamente", "Info", MessageBoxButtons.OK, MessageBoxIcon.Information);
                        this.Close();
                    }
                    else
                    {
                        MessageBox.Show("Ocurrio un error, por favor intentelo de nuevo", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
                    }
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message, "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }
    }
}
```
