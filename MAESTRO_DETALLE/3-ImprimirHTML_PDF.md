# IMPRIMIR FACTURA
Para esta practica se implementaran el uso de la funcion **window.print()** incluida en los navegadores como parte de las API de Javascript en conjunto de HTML.

- Documentacion: [window.print()](https://developer.mozilla.org/es/docs/Web/API/Window/print) 

Esta funcion se puede invocar desde cualquier accion o evento HTML, para el caso se utilizara el evento **onload**, que practicamente ejecuta la funcion deseada luego de cargar/renderizar el HTML.

- Documentacion: [onload](https://www.w3schools.com/jsref/event_onload.asp)

## PARTE 1 - Codificacion de Accion GET
Lo unico necesario a implementar es una accion de tipo **GET** en el controlador que retorne una **View()**.
```csharp
public ActionResult Details(long id)
{
    // Buscar datos de la venta
    Venta venta = ventaBL.ObtenerPorId(id);

    // Cargar datos virtuales y detalles
    venta.Empleado = new EmpleadoBL().ObtenerPorId(venta.IdEmpleado);
    venta.Cliente = new ClienteBL().ObtenerPorId(venta.IdCliente);
    venta.DetallesVenta = new DetalleVentaBL().Buscar(new DetalleVenta { IdVenta = venta.IdVenta });

    // Cargar productos
    new DetalleVentaBL().CargarProductoVirtual(venta.DetallesVenta);

    // Enviar datos de la venta
    return View(venta);
}
```

![image](https://github.com/user-attachments/assets/1b1e10c3-08db-43e2-ad73-ad0e7e7d9175)

**NOTAS:**
- La accion retorna un objeto de tipo VENTA, el cual contiene los datos de la Venta, Empleado que la facturo, Cliente al que se le vendio y Detalle de los productos adquiridos en la venta.

## PARTE 2 - Crear View y maquetar la Factura

**Paso 1:** Colocar el mouse sobre la acción **Details**, luego dar clic derecho y seleccionar **Agregar Vista.**

**Paso 2:** Seleccionar **Vista de MVC 5** y dar clic en **Agregar**.

**Paso 3:** Seleccionar plantilla de tipo **"Details"** y luego como clase modelo "Venta", y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/cb1b1f9b-8d5a-4529-b57e-83509e2e7cc1)

**Resultado:**

![image](https://github.com/user-attachments/assets/e4070c66-ee1f-47d9-8e02-da3cd11c8346)

**Paso 4:** Actualizar los titulos y agregar estilos de Bootstrap a la pagina HTML.

![image](https://github.com/user-attachments/assets/3fec866c-45e7-4dd8-8c43-8d9b5839bd0d)

**Paso 5:** Agregar evento **onload** para ejecutar la impresion del documento HTML y estilos css.

![image](https://github.com/user-attachments/assets/c30c3b6c-fac7-4073-b94b-372d292abf66)

**Paso 6:** Eliminar los datos no necesarios y actualizar el tratamiento de datos de llaves foraneas y campos autogenerados.

![image](https://github.com/user-attachments/assets/4d573d50-9630-4bb9-9b1a-710a3e651727)

**Resultado:**

![image](https://github.com/user-attachments/assets/837a5c3e-a496-413f-8268-17ea2c0e9b7c)

**Paso 7:** Agregar tabla para mostrar los detalles de productos adquiridos por el cliente.
```razor
<table class="table table-bordered table-striped">
    <thead>
        <tr>
            <th>Producto</th>
            <th>Cantidad</th>
            <th>Precio</th>
            <th>Subtotal</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var detalle in Model.DetallesVenta)
        {
            <tr>
                <td>@detalle.Producto.Nombre</td>
                <td>@detalle.Cantidad</td>
                <td>@detalle.Precio</td>
                <td>@detalle.Subtotal</td>
            </tr>
        }
    </tbody>
</table>
```

![image](https://github.com/user-attachments/assets/080c5de6-b6f9-451c-9857-34a9fd3316be)

**Resultado:**

![image](https://github.com/user-attachments/assets/efe8950e-969a-45e7-a34f-310d675faa99)

**Paso 8:** Mejorar estilos y presentacion de datos a conveniencia. Correccion de datos de divisas.

![image](https://github.com/user-attachments/assets/df6ea644-47d7-4ace-b45d-b5d1114cb930)

**Paso 9:** Modificar el enlace de **Ver Factura** en la vista **Index** del controlador de **Venta**.
```razor
@Html.ActionLink("Ver Factura", "Details", new { id = item.IdVenta }, new { @target = "_blank" })
```

![image](https://github.com/user-attachments/assets/a255eec9-f005-4ac2-b1df-0ff1ee6f833b)

**NOTA:**
- Esta modificacion abrira la factura en una pagina nueva.
- Documentacion [target](https://www.w3schools.com/tags/att_a_target.asp) del enlace en HTML.

**Paso 10:** Iniciar la aplicacion web en el IIS Express.

![image](https://github.com/user-attachments/assets/6eb19f1c-e99c-4215-bd75-479f6fd1307f)

**Paso 11:** Dar clic en el enlace **Ver Factura** de una Venta.

![image](https://github.com/user-attachments/assets/7df34747-fdfd-4388-aa5e-195168a7f0a9)

**Resultado:**

![image](https://github.com/user-attachments/assets/568e5889-1029-46a1-bc56-ebdabe217d69)

**Paso 12:** Detener la aplicacion web

![image](https://github.com/user-attachments/assets/cd85f0e0-8342-47b9-9426-ac8337b0cc59)
