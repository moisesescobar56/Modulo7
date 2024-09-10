# AUTENTICACION - FRONT-END

## PARTE 1 - LOGICA DE LA AUTENTICACION DE USUARIO
**¿Qué es Autenticación?**

Es el mecanismo que permite afirmar que la persona que esta ingresando al sistema es quien dice ser, mediante la validacion de crendeciales (usuario y contraseña) en la base de datos u otro servicio.

![image](https://github.com/user-attachments/assets/64a3dfc7-7ff4-461e-9ed1-b8eb7a39603b)

## PARTE 2 - DISEÑO DE FORMULARIO DE INICIO DE SESION 
**Paso 1:** Crear un nuevo formulario en el proyecto **"SistemaElParaisal.UI.WinForms", dar clic derecho y seleccionar **Agregar > Formulario (Windows Forms)**

![image](https://github.com/user-attachments/assets/31f7e489-a056-47ac-adef-91ed7396e2dc)

**Paso 2:** Nombramos el formulario como "InicioSesionForm.cs", luego dar clic en **Agregar**. 

![image](https://github.com/user-attachments/assets/4bc5dd5c-247e-4138-9de3-fe0ecd9bf288)

**Resultado:**
![image](https://github.com/user-attachments/assets/0942c624-3b22-42ff-8fed-979be59e0d5a)

**Paso 3:** Diseñar el formulario, agregando los controles para las credenciales (Telefono y Clave) y un boton para la accion **"Entrar"**.

![Login](https://github.com/user-attachments/assets/7045dc06-04ef-4e79-80d4-7409b2183cf1)

**Paso 4:** Activar **Password chars** para ocultar la contraseña/clave al escribirla. 

![image](https://github.com/user-attachments/assets/f1b7f6b8-adfc-4061-b9b6-b1efe64220a7)

**Paso 5:** Abrir el archivo Program.cs y cambiar el formulario de inicio, iniciar con **"InicioSesionForm"**.

![image](https://github.com/user-attachments/assets/a13e80f1-98a6-471b-98bf-fe94a91369f2)

**Resultado:**

![image](https://github.com/user-attachments/assets/b3b694b0-475a-4f06-803d-1cf84b1d88e8)

**Paso 6:** Iniciar la aplicacion.
![image](https://github.com/user-attachments/assets/c44bad4d-30e8-452e-bc00-594b96ab4a43)

**Resultado:**
![image](https://github.com/user-attachments/assets/fcb33fa9-5974-43ce-959c-ca62ae06000a)

**Paso 7:** Detener la aplicacion.
![image](https://github.com/user-attachments/assets/56e319f1-4420-4a3d-9c4f-e72252c891c3)

## PARTE 3 - CODIFICAR BOTON ENTRAR
**Paso 1:** Dar doble clic sobre el boton **"entrarButton" para generar el evento click.

![image](https://github.com/user-attachments/assets/3a80ea9f-6064-4f58-8d21-35045c744980)

**Resultado:**
![image](https://github.com/user-attachments/assets/e2a71c9f-6e3c-4d83-b08a-3cddcbd7d4a5)

**Paso 2:** Agregar las referencias a bilbiotecas al proyecto. 
```csharp
// Referencias del proyecto
using SistemaElParaisal.EN;
using SistemaElParaisal.BL;
```
![image](https://github.com/user-attachments/assets/15672b50-2561-48af-a0dd-112e5bbb5c1a)

**Paso 3:** Crear instancia de conexion a la base de datos mediante la clase de usuario (EmpleadoBL).
```csharp
// Conexion a la tabla de Empleados en la DB
EmpleadoBL empleadoBL = new EmpleadoBL();
```
![image](https://github.com/user-attachments/assets/875b9a34-ba7a-4c38-9561-b2de201a3e82)

**Paso 4:** Codificar la logica de autenticacion. 
```csharp
if (string.IsNullOrWhiteSpace(telefonoTextBox.Text) || string.IsNullOrWhiteSpace(claveTextBox.Text))
{
    // Validacion de datos
    MessageBox.Show("Por favor ingrese su telefono y clave", "Validacion", MessageBoxButtons.OK, MessageBoxIcon.Exclamation);
}
else
{
    // Capturar las credenciales 
    Empleado credencialesEmpleado = new Empleado { 
        Telefono = telefonoTextBox.Text, 
        Clave = claveTextBox.Text                    
    };
    // Autenticar credenciales en la base de datos
    Empleado objEmpleado = empleadoBL.AutenticarCredenciales(credencialesEmpleado);

    if (objEmpleado.IdEmpleado > 0)
    {
        // Acceso exitoso, el usuario tiene credenciales validas
        // Agregar logica
        MessageBox.Show("Inicio de sesion correcto", "Exito", MessageBoxButtons.OK, MessageBoxIcon.Information);
    }
    else
    {
        // Acceso denegado, el usuario no tiene credenciales validas
        MessageBox.Show("Las credenciales ingresadas no son validas", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
    }
}
```

![image](https://github.com/user-attachments/assets/c03f819e-52e8-411f-8837-f96d8bc479d3)


**Paso 5:** Iniciar la aplicacion.
![image](https://github.com/user-attachments/assets/c44bad4d-30e8-452e-bc00-594b96ab4a43)

**Paso 6:** Ingresar credenciales validas y dar clic en el boton **"ENTRAR"**.
![image](https://github.com/user-attachments/assets/667d7bd5-229a-4e37-b89c-0e145e493f50)

**Resultado:** Dar clic en **Aceptar**.

![image](https://github.com/user-attachments/assets/b454d43c-2b2e-4aed-8092-0ac7949c77a3)


**Paso 7:** Detener la aplicacion.
![image](https://github.com/user-attachments/assets/56e319f1-4420-4a3d-9c4f-e72252c891c3)
