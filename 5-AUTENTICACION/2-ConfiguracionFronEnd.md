# AUTENTICACION - FRONT-END

## PARTE 1 - LOGICA DE LA AUTENTICACION DE USUARIO
**¿Qué es Autenticación?**

Es el mecanismo que permite afirmar que la persona que esta ingresando al sistema es quien dice ser, mediante la validacion de crendeciales (usuario y contraseña) en la base de datos u otro servicio.

![image](https://github.com/user-attachments/assets/64a3dfc7-7ff4-461e-9ed1-b8eb7a39603b)

## PARTE 2 - Crear y Configurar AuthController
**Paso 0:** Ubicarse en el proyecto **SistemaVentas.UI.AppWebMVC**. 

**Paso 1:** Seleccionar l
a carpeta **Controllers** luego dar clic derecho y seleccionar **Agregar > Controlador**.

![image](https://github.com/user-attachments/assets/08edc07c-3beb-4a37-a1e4-06a3d20e8ec0)

**Paso 2:** Seleccionar “Controlador de MVC5: en blanco” y dar clic en Agregar.

![image](https://github.com/user-attachments/assets/295748ef-4aa5-4ff3-9047-525da2c6ec31)

**Paso 3:** Ingresar el nombre **AuthController** y dar clic en **Agregar**.
```
AuthController
```

![image](https://github.com/user-attachments/assets/68f39255-9581-4d08-8848-a2d7152cf81e)

**Resultado:**
![image](https://github.com/user-attachments/assets/3360fcb0-cf8b-47ec-8238-66f92dfb1cb0)

**Paso 4:** Agregar referencias e instancia de **EmpleadoBL** en el **AuthController**.
```csharp
// Referencias
using SistemaElParaisal.EN;
using SistemaElParaisal.BL;
using System.Web.Security;
```
```csharp
 //Conexion a la tabla Empleado mediante EmpleadoBL
 EmpleadoBL empleadoBL = new EmpleadoBL();
```

![image](https://github.com/user-attachments/assets/9b6f88ac-be14-4fc9-8ff2-ae885223149d)

**Paso 5:** Agregar el parametro **mensaje** y programar lógica de la acción Index (GET) en el **AuthController**.

```csharp
string mensaje = null
```
```csharp
ViewBag.mensaje = mensaje;

FormsAuthentication.SignOut();

return View( new Empleado() );
```

![image](https://github.com/user-attachments/assets/349d9e6f-4be5-477b-a3bd-bacf3ed6e530)

**NOTAS:**
-**ViewBag.mensaje** retorna un dato a la vista Index.
- **FormsAuthentication.SignOut()** se encarga de cerrar la sesion activa en la aplicacion web.
- **return View( new Empleado() )** retorna una vista con una nueva instancia vacia de la clase que registra a los usuarios del proyecto.

**Paso 6:** Programar lógica de la acción **Index** (POST) en el **AuthController**.

```csharp
[HttpPost]
public ActionResult Index(Empleado pEmpleado)
{
    if (pEmpleado == null) 
    { 
        return View( new Empleado() ); 
    }
    // Validar credenciales del usuario a autenticar
    Empleado empleadoSesion = empleadoBL.AutenticarCredenciales(pEmpleado);

    if (empleadoSesion != null && empleadoSesion.IdEmpleado > 0)
    {
        // Guardar en sesion los datos del Empleado
        Session["usuario"] = empleadoSesion;

        // Obtener y guardar en sesion el nivel de acceso del empleado
        Cargo cargo = new CargoBL().ObtenerPorId(empleadoSesion.IdCargo);
        Session["cargo"] = cargo.Nombre;

        // Guardar la sesion del empleado
        FormsAuthentication.RedirectFromLoginPage(empleadoSesion.Telefono, false);
        return RedirectToAction("Index","Home");
    }
    else
    {
        ViewBag.mensaje = "Credenciales incorrectas";
    }

    return View(pEmpleado);
}
```

**NOTAS:**
- **Session["key"]** permite crear una variable almacenada de forma global en la memoria de la aplicacion, siempre y cuando la sesion este activa.
- **FormsAuthentication.RedirectFromLoginPage** activa la autenticacion(sesion) del usuario en el servidor.

![image](https://github.com/user-attachments/assets/4b6909d9-c1b0-4fc2-b815-cb64b369ec91)

**Paso 7:** Programar lógica de la acción **CerrarSesion** (GET) en el **AuthController**.

```csharp
  public ActionResult CerrarSesion()
  {
      Session.Abandon();
      FormsAuthentication.SignOut();
      return RedirectToAction("Index", "Auth");
  }
```

![image](https://github.com/user-attachments/assets/959e9985-e2b9-4aeb-9f7d-2986641bfc70)

**NOTAS:**
- **Session.Abandon()** vacia de la memoria/abandona la sesion del servidor.

## PARTE 3 - Diseño de la vista de inicio de sesión y configuración

**Paso 1:** Dar clic derecho sobre la acción Index (GET) en el **AuthController** y seleccionar **Agregar
Vista**.

![image](https://github.com/user-attachments/assets/2e67c3af-cb0b-4b47-baf9-db845d61cf0d)

**Paso 2:** Seleccionar **Vista de MVC 5** y dar clic sobre **Agregar**.

![image](https://github.com/user-attachments/assets/f72ea193-291d-4864-b1e8-a696eb6e1c2f)


**Paso 3:** verificar que la pagina de diseño este seleccionada y dar clic sobre Agregar.

```
NOTA: Si se desea crear un diseño si incluir en menu de navegacion del layout, desmarque la opcion "Usar pagina de diseño"
```

![image](https://github.com/user-attachments/assets/29c524ac-8d43-4e0e-977e-26826aa89d63)

**Resultado:** 

![image](https://github.com/user-attachments/assets/9fe52b31-4770-41ae-b26b-d8b2801c98fe)


**Paso 4:** Agregar el **modelo** de **Empleado** manualmente a la vista. El **model** solo se usara para volver a
mostrar el telefono ingresado al usuario en caso ingrese credenciales incorrectas.

```razor
@model SistemaElParaisal.EN.Empleado
```

![image](https://github.com/user-attachments/assets/40d1ea70-2df7-4375-8312-e61068f37569)

**Paso 5:** Maquetar y diseñar formulario básico de inicio de sesión.

```razor
@if (ViewBag.mensaje != null)
{
    <div class="alert alert-danger">@ViewBag.mensaje</div>
}

<form method="post" action="@Url.Action("Index","Auth")" class="d-inline-flex">

    <input type="text" class="form-control" placeholder="Teléfono" 
           name="Telefono" value="@Model.Telefono" required />

    <input type="password" class="form-control mx-3" placeholder="Contraseña" 
           name="Clave" required />

    <button type="submit" class="btn btn-primary">Entrar</button>

</form>
```

![image](https://github.com/user-attachments/assets/bb6d1021-d3c2-4697-baee-8d8b75959698)


**NOTAS:**
- **@Url.Action("Action", "Controller")** define el controlador donde enviara el formulario los datos mediante el **Protocolo HTTP POST**.
- **Credenciales** para este ejemplo, las credenciales de los usuarios se componen de **Telefono y Clave**.

**NOTA IMPORTANTE:**

El **name** de los **inputs** de las credenciales (telefono y clave), deben coincidir con los de la
clase de **“autenticación”** de nuestro proyecto, en este caso la clase es **Empleado**.

---

**Paso 6:** Abrir la vista **Index** del controlador **Home**.

**Paso 7:** Agregar el **using** de la capa **Entidades (EN)** del proyecto mediante razor.

```razor
@using SistemaElParaisal.EN
```

![image](https://github.com/user-attachments/assets/ebd811d0-e9ba-465e-a4a2-9400afa68ccc)

**Paso 8:** Crear un método para capturar el nombre completo del usuario actual en sesión.

```razor
string ObtenerNombreEmpleado()
{
    if (Session["usuario"] != null)
    {
        Empleado empleado = (Empleado)Session["usuario"];
        return empleado.Nombre + " " + empleado.Apellido;
    }
    return "";
}
```

![image](https://github.com/user-attachments/assets/317a9655-6682-40b5-9ef7-03785fbd0097)

**NOTA:** Para este ejemplo, se captura los datos del usuario de la variable global de **Session["usuario"]** y se convierte al tipo de clase que era en un inicio. 

**Paso 9:** Invocamos el método **ObtenerNombreEmpleado()** para imprimir en pantalla un saludo de bienvenida.
```razor
@if (ObtenerNombreEmpleado() != "")
{
    <p>Bienvenido @ObtenerNombreEmpleado()</p>
}
```

![image](https://github.com/user-attachments/assets/74013a8d-0194-4fde-8a4c-82a71e3383b4)

**Paso 10:** Abrir el archivo **Web.config** del proyecto ASP.NET para habilitar la autenticación mediante Formularios (Forms).

```
OJO: prestar atencion al archivo que se solicita en la imagen.
```

![image](https://github.com/user-attachments/assets/01cbd952-ca0c-4724-a74a-15eaca7f335d)

**Paso 11:** Editar el archivo **Web.config**, ubicar la sección **<system.web>... </system.web>** y abrir un
espacio debajo de la configuración **<httpRuntime ... />**

![image](https://github.com/user-attachments/assets/b20cc7af-1200-48c1-bffe-e1c21049ee78)

**Paso 12:** Agregar la siguiente configuración al archivo **Web.config**.

```xml
<!--Auth-->
<authentication mode="Forms">
  <forms loginUrl="~/Auth/Index" timeout="4500"></forms>
</authentication>
<!-- Fin Auth -->
```

![image](https://github.com/user-attachments/assets/fd7aca51-5b08-438b-8161-f86b902dc127)

**Paso 13:** Iniciar el servidor IIS. 

![image](https://github.com/user-attachments/assets/6eb19f1c-e99c-4215-bd75-479f6fd1307f)

**Paso 14:** Acceder al formulario de autenticación ingresando a la url **https://servidor/Auth** como se muestra en la imagen. Luego presionar la tecla **Enter**.

![image](https://github.com/user-attachments/assets/71764da8-bab0-4d49-874f-d9293d5ac54f)


**Resultado:**

![image](https://github.com/user-attachments/assets/285a5b82-fc89-4696-ab22-8e44b3bc3838)


**Paso 15:** Ingresar las credenciales de un empleado de la base de datos y dar clic en **Entrar**.

![image](https://github.com/user-attachments/assets/6ea46b04-39f2-40ef-a419-132cfc6c27c2)

![image](https://github.com/user-attachments/assets/1f12cf08-fd36-485c-9400-42a64ac381d6)

**Resultado:**

![image](https://github.com/user-attachments/assets/e5e53fdf-7249-439c-a2da-dfac3d58c0a3)

**Nota:** Si el usuario se equivoca en las credenciales, recibirá un mensaje igual al siguiente.

![image](https://github.com/user-attachments/assets/b1fe8b20-ad74-454a-bdb2-42e821dafdc8)

---
#### SEGURIDAD
De momento la aplicación ya cuenta con un sistema de autenticación parcial, si nos damos cuenta en la siguiente imagen cualquier usuario puede ver las opciones del menú lateral.

![image](https://github.com/user-attachments/assets/cfcbf61c-a39e-4436-87f3-a981d6ffb54d)

---

**Paso 16:** Detener el servidor IIS.
![image](https://github.com/user-attachments/assets/cd85f0e0-8342-47b9-9426-ac8337b0cc59)
