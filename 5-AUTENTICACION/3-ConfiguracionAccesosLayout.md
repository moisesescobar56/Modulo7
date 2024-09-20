![image](https://github.com/user-attachments/assets/402fbb3e-577a-477b-9534-4562cd0faf72)# CONFIGURACION DE LAYOUT SEGUN NIVEL DE ACCESO 
#### SEGURIDAD
De momento la aplicación ya cuenta con un sistema de autenticación parcial, si nos damos cuenta en la siguiente imagen cualquier usuario puede ver las opciones del menú lateral.

![image](https://github.com/user-attachments/assets/cfcbf61c-a39e-4436-87f3-a981d6ffb54d)

---

# PARTE 1 - Actualización de seguridad de accesos en el Layout

**Paso 1:** Abrir el archivo **_Layout**.

![image](https://github.com/user-attachments/assets/44d1647b-1317-4e23-8d58-3855b852d5a4)

**Paso 2:** Crear método **ObtenerCargo()** y **ObtenerNombreEmpleado()** al inicio del Layout para obtener el cargo y el nombre del usuario en sesión.
```razor
@using SistemaElParaisal.EN

@{
    string ObtenerCargo()
    {
        return Session["cargo"] != null ? Session["cargo"].ToString().ToUpper() : "";
    }

    string ObtenerNombreEmpleado()
    {
        if (Session["usuario"] != null)
        {
            Empleado empleado = (Empleado)Session["usuario"];
            return empleado.Nombre + " " + empleado.Apellido;
        }
        return "-";
    }
}
```

![image](https://github.com/user-attachments/assets/b4a30d68-d913-4f03-b167-ffad10d307b6)

**Paso 3:** Ubicarse en la etiqueta <ul> del **NavBar** para eliminar los enlaces.

![image](https://github.com/user-attachments/assets/6a527a68-e1e8-412b-82f1-55d12a664db9)

**Resultado:**
![image](https://github.com/user-attachments/assets/8ea151b4-239a-4b0d-a00a-59e81c592251)

**Paso 4:** Habilitar opción de inicio de sesión y salir del sistema, configurando los enlaces correspondientes.
```razor
@if (User.Identity.IsAuthenticated)
{
   <li>
       <a class="dropdown-item" href="@Url.Action("View", "Controller")">
           <i class="fa fa-gear"></i>  Configuración
       </a>
   </li>
   <li><hr class="dropdown-divider" /></li>
   <li>
       <a class="dropdown-item" href="@Url.Action("CerrarSesion", "Auth")">
           <i class="fa fa-power-off"></i> Cerrar Sesión
       </a>
   </li>
}
else
{
   <li>
       <a class="dropdown-item" href="@Url.Action("Index", "Auth")">
           <i class="fa fa-right-to-bracket"></i>  Iniciar Sesión
       </a>
   </li>
}
```

![image](https://github.com/user-attachments/assets/70674b62-3128-460d-81a1-2ad5438dedc4)

**NOTA:** **User.Identity.IsAuthenticated** identifica si hay un usuario autenticado, si lo hay devuelve **true** y sino **false**. 

**Paso 5:** Abrir un espacio al inicio del **div** con la clase **nav** ubicado en la seccion **"Lateral Nav"**.

![image](https://github.com/user-attachments/assets/066e675b-7667-48c4-916f-84dd621b03a1)


**Paso 6:** Agregar la validación de acceso según cargo del usuario.

```razor
@if (ObtenerCargo() == "ADMINISTRADOR")
{

}
else if (ObtenerCargo() == "VENDEDOR")
{

}
```

![image](https://github.com/user-attachments/assets/57945933-e304-414a-9827-cdbee557c679)

**Paso 7:** Agregar los links de acceso según el cargo del usuario.

![image](https://github.com/user-attachments/assets/7fd0e275-fd37-47dc-a26a-9f359a742db0)

Según la validación anterior, el **Administrador** podrá acceder desde el menú a los links de accesos de **Empleados** y **Productos**, y los usuarios de otros cargos no, ejemplo el **Vendedor** solo tiene acceso a **Productos**, sin embargo todos los cargos deben poder iniciar sesión en el sistema inicialmente.

**Paso 8:** Remplazar la palabra **Usuario** en footer del **"Lateral Nav"**, para mostrar el nombre del usuario activo.
```razor
@ObtenerNombreEmpleado()
```
![image](https://github.com/user-attachments/assets/e4aa90c3-0da2-42db-8e4b-d5fdc3dfea07)

**Resultado:**

![image](https://github.com/user-attachments/assets/fc549529-8909-4129-9cd9-16c56041a9ce)

**Paso 9:** Dar clic en iniciar el **IIS Express** y verificar que la actualización del Layout se aplico.

![image](https://github.com/user-attachments/assets/6eb19f1c-e99c-4215-bd75-479f6fd1307f)

**Resultado:** ahora podemos observar que no podemos ver los links de acceso.

![image](https://github.com/user-attachments/assets/2e50be48-f0d1-4e68-89ec-b769b2c977d6)

**Paso 10:** Dar clic en Iniciar sesión y probar ingresar al sistema con todas las credenciales de los usuarios:

![image](https://github.com/user-attachments/assets/6ea46b04-39f2-40ef-a419-132cfc6c27c2)

![image](https://github.com/user-attachments/assets/a8b3166d-c84c-4c13-90f1-57289f20d9ed)

![image](https://github.com/user-attachments/assets/3cc20185-79ef-42a3-9d30-5ebb61c30a22)

#### Resultado: Administrador

![image](https://github.com/user-attachments/assets/73baed4b-8164-475f-9faf-491f43e62296)

**Nota:** Observemos que tenemos accesos a los links configurados de Empleados y Productos.

**Paso 11:** Damos clic en la opción **Salir**.

![image](https://github.com/user-attachments/assets/34954729-c515-43eb-bd0f-cfe24ef7b61c)

#### Resultado: Vendedor

![image](https://github.com/user-attachments/assets/b9e13c2c-4ecd-40a8-b37c-77bdbecb7689)

![image](https://github.com/user-attachments/assets/2e718a53-1754-4a41-b4ec-bf280d3e501f)

**Nota:** Observemos que no tenemos accesos a los links configurados de Empleados, solo Productos.

**Paso 12:** Detener la aplicación web.

![image](https://github.com/user-attachments/assets/cd85f0e0-8342-47b9-9426-ac8337b0cc59)
