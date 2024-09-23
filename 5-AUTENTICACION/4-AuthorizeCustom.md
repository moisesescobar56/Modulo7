# Implementación de seguridad de acceso en controladores mediante [Decoradores].
La siguiente implementación protege las acciones dentro de los controladores, para evitar el acceso o invocación de acciones de forma manual en la aplicación web sin haberse autenticado o poseer un usuario dentro del sistema.

**Paso 0:** Ubicarse en el proyecto SistemaElParaisal.UI.AppWebMVC. 

**Paso 1:** Seleccionar la carpeta **App_Start** luego dar clic derecho y seleccionar **Agregar > Clase**.

**Paso 2:** Ingresar el nombre **AuthorizeCustom** y dar clic en Agregar.
```
AuthorizeCustom
```


**Resultado:**

**Paso 3:** Borrar el nombre de la carpeta **App_Start** del espacio de nombres, para globalizar la clase.

**Resultado:**

**Paso 4:** Agregar referencias y herencia de **AuthorizeAttribute** a la clase **AuthorizeCustom**. 


**Paso 5:** Agregar los decoradores heredados de **AuthorizeAttribute**, a la clase **AuthorizeCustom**.


**Paso 6:** Agregar variables locales y constructor que reciba los nombres de los roles que tiene acceso a las acciones o controladores.



**Paso 7:** Crear **override** (sobrescribir) del método **OnAuthorization** para validar según nuestros cargos el acceso a los controladores o acciones.


**Paso 8:** Programar la lógica del **override** del método **OnAuthorization**.

**Paso 9:** Abrir un controlador e implementar la seguridad mediante un decorador. Ejemplo: **EmpleadoController**.


**Paso 10:** Implementar el decorador **[AuthorizeCustom]** en el **EmpleadoController**. 

## Todos los cargos: en este ejemplo se está dando acceso a todos los cargos.


## Solo un cargo: en este ejemplo se está dando acceso solo a usuario con cargo de administrador.

**Paso 11:** Dar clic en iniciar el **IIS Express** y probar el funcionamiento de la seguridad.
![image](https://github.com/user-attachments/assets/6eb19f1c-e99c-4215-bd75-479f6fd1307f)

**Paso 12:** Si en la barra de direcciones agregamos “/Empleado” para acceder al controlador de empleados sin iniciar sesión, como se muestra en la imagen y pulsamos la tecla Enter.

**Resultado:** El servidor nos enviara al inicio de sesión indicando que el acceso es denegado.


**Paso 13:** Detener el servidor IIS.
![image](https://github.com/user-attachments/assets/cd85f0e0-8342-47b9-9426-ac8337b0cc59)
