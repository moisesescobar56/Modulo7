# Implementación de seguridad de acceso en controladores mediante [Decoradores].
La siguiente implementación protege las acciones dentro de los controladores, para evitar el acceso o invocación de acciones de forma 
manual en la aplicación web sin haberse autenticado o poseer un usuario dentro del sistema.

- [Documentacion Miscrosoft - AuthorizeAttribute](https://learn.microsoft.com/es-es/dotnet/api/system.web.mvc.authorizeattribute?view=aspnet-mvc-5.2)

**Paso 0:** Ubicarse en el proyecto SistemaElParaisal.UI.AppWebMVC. 

**Paso 1:** Seleccionar la carpeta **App_Start** luego dar clic derecho y seleccionar **Agregar > Clase**.

![image](https://github.com/user-attachments/assets/71f5d155-f859-4a7f-a0d5-78a87786f9c2)

**Paso 2:** Ingresar el nombre **AuthorizeCustom** y dar clic en Agregar.
```
AuthorizeCustom
```
![image](https://github.com/user-attachments/assets/dab55ad6-6e67-4dc5-8767-b87916a1d6c6)

**Resultado:**
![image](https://github.com/user-attachments/assets/bb90cbb3-b6db-4c12-bb1d-17bb51fdfe8e)

**Paso 3:** Borrar el nombre de la carpeta **App_Start** del espacio de nombres, para globalizar la clase.

![image](https://github.com/user-attachments/assets/7e929567-3f07-4d67-9458-2e06d9d970ed)

**Resultado:**

![image](https://github.com/user-attachments/assets/fd7fc7c6-9c43-4fdb-ae4a-c92cec76e30b)

**Paso 4:** Agregar referencias y herencia de **AuthorizeAttribute** a la clase **AuthorizeCustom**. 

```csharp
// Referencias
using System.Web.Mvc;
```
```csharp
AuthorizeAttribute
```

![image](https://github.com/user-attachments/assets/4cb5cfba-962f-4b14-85e1-eeea33334487)

**Paso 5:** Agregar los decoradores heredados de **AuthorizeAttribute**, a la clase **AuthorizeCustom**.
```csharp
[AttributeUsage(AttributeTargets.All, AllowMultiple = true)]
```
![image](https://github.com/user-attachments/assets/8bc6c6ff-712c-4483-8cac-077d10853ffb)


**Paso 6:** Agregar variables locales y constructor que reciba los nombres de los roles que tiene acceso a las acciones o controladores.

```csharp
// atributos
private string cargoSesion;
private string nombresCargo;

public AuthorizeCustom(string pNombresCargo)
{
    // constructor
    nombresCargo = pNombresCargo;
}
```

![image](https://github.com/user-attachments/assets/04440897-00b7-4948-ae56-8e1ac0624d2f)

**NOTAS:**
- **cargoSesion:** se utilizara para extraer el nombre del cargo del usuario en sesion, a partir de la variables de sesion **Session["cargo"]**.
- **nombresCargo:** se utilizara para guardar en memoria el nombre de los cargos validos en cada peticion a los controladores.
---
**Paso 7:** Crear **override** (sobrescribir) del método **OnAuthorization** para validar según nuestros cargos el acceso a los controladores o acciones.

```csharp
public override void OnAuthorization(AuthorizationContext filterContext)
{
    
}
```

![image](https://github.com/user-attachments/assets/7b136d6e-5643-406f-9c95-0dbc6516c9ec)

**Paso 8:** Programar la lógica del **override** del método **OnAuthorization**.

```csharp
try
{
    cargoSesion = (string)HttpContext.Current.Session["cargo"];
    if (cargoSesion == null)
    {
        // No ha inicio sesion el usuario, redireccionar
        filterContext.Result = new RedirectResult("~/Auth/Index?mensaje=401 - Acceso denegado");
    }
    else
    {
        // Existe un usuario en sesion, verificar nivel acceso del cargo
        if (nombresCargo.ToUpper().Contains(cargoSesion.ToUpper()) == false)
        {
            // Usuario no tiene acceso, redireccionar
            filterContext.Result = new RedirectResult("~/Auth/Index?mensaje=401 - Acceso denegado");
            HttpContext.Current.Session.Abandon();
        }
    }
}
catch (Exception ex)
{
    // error interno en el servidor
    filterContext.Result = new RedirectResult("~/Auth/Index?mensaje=500 - Error interno en el servidor");
}
```

![image](https://github.com/user-attachments/assets/0896be1e-5602-4bde-bbea-db2733b8f6d4)

**Paso 9:** Abrir un controlador e implementar la seguridad mediante un decorador. Ejemplo: **EmpleadoController**.

![image](https://github.com/user-attachments/assets/0666ec4a-48fb-42ca-ab3c-dde12e1dd083)

**Paso 10:** Implementar el decorador **[AuthorizeCustom]** en el **EmpleadoController**. 

## TODOS LOS CARGOS: en este ejemplo se está dando acceso al controlador a todos los cargos. Los nombres de los cargos estan separados por una linea vertical **"|"**.

```csharp
[AuthorizeCustom("ADMINISTRADOR|VENDEDOR")]
```

![image](https://github.com/user-attachments/assets/2b837858-bc4c-4c19-8c94-d411a260de77)


## SOLO UN CARGO: en este ejemplo se está dando acceso al controlador solo a los usuarios con cargo de Administrador.

```csharp
[AuthorizeCustom("ADMINISTRADOR")]
```
![image](https://github.com/user-attachments/assets/92488ce2-eefa-4fe6-b077-e4b92a7934a3)

---
**NOTA:** Los cargos deben ser nombres que existan en nuestra tabla de **cargos/roles/permisos**.

![image](https://github.com/user-attachments/assets/ceff48a7-51a6-4789-8436-7e77ccd7e804)

---
**Paso 11:** Dar clic en iniciar el **IIS Express** y probar el funcionamiento de la seguridad.
![image](https://github.com/user-attachments/assets/6eb19f1c-e99c-4215-bd75-479f6fd1307f)

**Paso 12:** Si en la barra de direcciones agregamos “/Empleado” para acceder al controlador de empleados sin iniciar sesión, como se muestra en la imagen y pulsamos la tecla Enter.

![image](https://github.com/user-attachments/assets/a8eb2b5a-7f71-4fb2-98eb-e9c60e2e6d26)

**Resultado:** El servidor nos enviara al inicio de sesión indicando que el acceso es denegado.

![image](https://github.com/user-attachments/assets/da6348a9-f0e1-4c69-849e-f71c0bba8a3d)

**Paso 13:** Detener el servidor IIS.
![image](https://github.com/user-attachments/assets/cd85f0e0-8342-47b9-9426-ac8337b0cc59)
