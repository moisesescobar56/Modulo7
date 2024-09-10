# Programación de funcionalidad BUSCAR

## PARTE 1 - Configurar Referencias y Conexión
**Paso 1:** Seleccionar el formulario y en la ventana de Propiedades dar clic sobre el menú de **Eventos**. 

![image](https://github.com/user-attachments/assets/2d129675-cdd7-4457-b40e-2ba6160a0c97)

**Paso 2:** En el evento Load dar doble clic en el espacio en blanco para generar el evento Load del 
formulario.

![image](https://github.com/user-attachments/assets/7b970327-3109-42bf-b953-a83e2427a50b)

**Resultado:**
![image](https://github.com/user-attachments/assets/3e10c566-8e1f-4ca7-b49a-c2a9d3279ef0)

**Paso 3:** Agregar referencias a librerías en el formulario.
```csharp
//Referencias del proyecto
using SistemaElParaisal.EN;
using SistemaElParaisal.BL;
```
![image](https://github.com/user-attachments/assets/782a82f6-78ea-4bc1-8f8e-fe452b9d0e21)

**Paso 4:** Agregar crear conexión a la base de datos mediante EmpleadoBL y agregar variable “lista” para cargar los empleados en el DataGridView. 
```csharp
// Conexion a la tabla de Empleados en la DB
EmpleadoBL empleadoBL = new EmpleadoBL();
//Variables
List<Empleado> lista = new List<Empleado>();
```
![image](https://github.com/user-attachments/assets/adc667d2-1f66-4eca-a48c-25b18420eea7)

## PARTE 2 - Codificacion de ComboBox (Lista de selección de Cargos)

![image](https://github.com/user-attachments/assets/d620e7af-286c-4dc6-8f00-09791f5658ab)

**Paso 1:** Programar la lógica del método interno "CargarCargos()" para mostrar las cargos en forma de lista de selecciónen el control cargoComboBox.
```csharp
public void CargarCargos()
{
    // Conexion a la tabla de Cargo en la DB
    CargoBL cargoBL = new CargoBL();

    // Inicializar lista 
    List<Cargo> cargos = new List<Cargo>();
    cargos.Add(new Cargo { IdCargo = 0, Nombre = "SELECCIONAR" }); // Opcion por defecto

    // Obtener lista de Cargos de la DB
    cargos.AddRange(cargoBL.Buscar(new Cargo()));
    cargoComboBox.DataSource = cargos;

    // Configurar texto y valor de la lista de seleccion
    cargoComboBox.DisplayMember = "Nombre";
    cargoComboBox.ValueMember = "IdCargo";
}
```
![image](https://github.com/user-attachments/assets/1c94811c-0855-4948-afef-71d8842e2ade)

- **DisplayMember:** es la propiedad del objeto VISIBLE para el usuario, representa el texto que muestra cada elemento. 
-  **ValueMember:** es la propiedad del objeto OCULTA para el usuario, que representa el valor de un elemento de la lista.

**Paso 2:** Codificar el evento Load del formulario. 
```csharp
//Cargar ComboBoxs en el formulario
CargarCargos();
```
![image](https://github.com/user-attachments/assets/1af6cf63-5b09-423a-be90-23d0f228f0f1)

**Paso 3:** Iniciar la aplicacion.
![image](https://github.com/user-attachments/assets/c44bad4d-30e8-452e-bc00-594b96ab4a43)

**Resultado:**
![image](https://github.com/user-attachments/assets/c1447221-bd12-4176-94b7-b480b91b56db)

**Paso 4:** Detener la aplicacion.
![image](https://github.com/user-attachments/assets/56e319f1-4420-4a3d-9c4f-e72252c891c3)

## PARTE 3 - Codificacion de Accion BUSCAR

**Paso 1:** Seleccionar el botón "buscarButton" y dar **doble clic** para generar evento click

![image](https://github.com/user-attachments/assets/d246b0c8-6dab-44bd-b56b-1786e157b4c0)

**Resultado:**
![image](https://github.com/user-attachments/assets/37722a63-4fc3-431e-82c2-f0a13c4c50db)

**Paso 2:** Programar la lógica del método interno CargarLista para cargar mostrar los empleados en forma de tabla en el control **listaDataGridView**.
```csharp
private void CargarLista()
{
    listaDataGridView.DataSource = ""; // Vaciar DataGridView
    listaDataGridView.DataSource = lista; // Agregar objetos de lista
}
```
![image](https://github.com/user-attachments/assets/ff5e08f1-a298-47cd-b73b-62fff788eda8)

**Paso 3:** Programar el evento click de **"buscarButton"**, agregando la siguiente lógica. 
```csharp
// Obtener los filtros de busquedas
Empleado empleado = new Empleado();
empleado.Nombre = nombreTextBox.Text;
empleado.Telefono = telefonoTextBox.Text;
empleado.IdCargo = (byte)cargoComboBox.SelectedValue;

// Ejecutar busqueda
lista = empleadoBL.Buscar(empleado);
CargarLista();
```

![image](https://github.com/user-attachments/assets/c1c31158-13a1-44ba-9f86-29377b3cb518)

**Paso 4:** Iniciar la aplicacion.
![image](https://github.com/user-attachments/assets/c44bad4d-30e8-452e-bc00-594b96ab4a43)

**Resultado:** Dar clic en el botón **BUSCAR** y se mostrara la lista de empleados guardados.
![image](https://github.com/user-attachments/assets/9933a892-4d30-4867-9fcb-6cec078ef017)

#### MEJORAS: No se muestra el nombre del cargo en los empleados filtrados por el cargo **“Administrador”**.
![image](https://github.com/user-attachments/assets/866a03fa-be78-4180-8d80-63c21aee01ec)

***NOTA:*** *Al observar el resultado, la información de empleados se carga correctamente, pero los datos
no se muestran de una forma limpia y comprensible para el usuario, debida a que el usuario 
no puede interpretar cual es el cargo con valor “1”*

**Paso 5:** Detener la aplicacion.
![image](https://github.com/user-attachments/assets/56e319f1-4420-4a3d-9c4f-e72252c891c3)


## REVISAR EL MATERIAL: "TRATAMIENTO DE DATOS"

