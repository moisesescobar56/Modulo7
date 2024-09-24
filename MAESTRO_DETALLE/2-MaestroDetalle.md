# Diseño y programación de Ventas

## PARTE 1 - Prototipo

![image](https://github.com/user-attachments/assets/0276c8b3-3447-4cb6-b0fc-cabbf3830ed1)


## PARTE 2 - Programación del Controlador de Venta

**Paso 0:** Ubicarse en el proyecto **SistemaELParaisal.UI.AppWebMVC**. 

**Paso 1:** Dar clic derecho sobre la carpeta Controllers y luego seleccionar **Agregar > Controlador**.

**Paso 2:** Seleccionar **Controlador de MVC5: en blanco** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/54a828c4-e8c6-44e2-a804-fea5ed089aed)

**Paso 3:** Ingresar el nombre **VentaController** y dar clic en **Agregar**.

**Resultado:**

![image](https://github.com/user-attachments/assets/d5c79540-aba2-4d37-94ed-c1a67916e805)

**Paso 4:** Agregar las referencias a bibliotecas e instancia a la tabla Venta desde VentaBL.
```csharp
// Referencias
using SistemaElParaisal.EN;
using SistemaElParaisal.BL;
```
```csharp
//Conexion a la tabla Venta
VentaBL ventaBL = new VentaBL();
```

![image](https://github.com/user-attachments/assets/ff896641-c1b0-45b0-a457-b03637527ad5)

**Paso 5:** Programar el **DropDownListEmpleado()** en VentaController para cargar una lista de Empleados.

```csharp
public static List<SelectListItem> DropDownListEmpleado(short pId = 0)
{
    List<Empleado> lista = new List<Empleado>();
    List<SelectListItem> options = new List<SelectListItem>
    {
        new SelectListItem { Value = null, Text = "Seleccionar" }
    };

    lista = new EmpleadoBL().Buscar(new Empleado { });

    options.AddRange(lista.OrderBy(x => x.Nombre).Select(x => new SelectListItem
    {
        Value = x.IdEmpleado.ToString(), // PK
        Text = x.Nombre + " " + x.Apellido,
        Selected = (x.IdEmpleado == pId),
    }).ToList());

    return options;
}
```

![image](https://github.com/user-attachments/assets/f80daba6-7a5c-4fb1-b2df-18fa4505a6a1)


**Paso 6:** Programar el **DropDownListCliente()** en VentaController para cargar una lista de Clientes.

```csharp
public static List<SelectListItem> DropDownListCliente(int pId = 0)
{
    List<Cliente> lista = new List<Cliente>();
    List<SelectListItem> options = new List<SelectListItem>
    {
        new SelectListItem { Value = null, Text = "Seleccionar" }
    };

    lista = new ClienteBL().Buscar(new Cliente { });

    options.AddRange(lista.OrderBy(x => x.Nombre).Select(x => new SelectListItem
    {
        Value = x.IdCliente.ToString(), // PK
        Text = x.Nombre + " " + x.Apellido,
        Selected = (x.IdCliente == pId),
    }).ToList());

    return options;
}
```

![image](https://github.com/user-attachments/assets/efd37e5b-4ef9-47df-a431-6b130b7f1e13)

**Paso 7:** Programar el metodo **ListaProductos()** en VentaController para cargar una lista de Productos.

```csharp
public List<Producto> ListaProductos(byte pIdCategoria = 0)
{
    List<Producto> lista = new List<Producto>();
    lista = new ProductoBL().Buscar(new Producto { IdCategoria = pIdCategoria });
    lista = lista.OrderBy(x => x.Nombre).ToList();
    return lista;
}
```

![image](https://github.com/user-attachments/assets/1b5e2bf1-fdc8-44d7-9ca1-2052e2c20738)


**Paso 8:** Programar la accion **Index** de VentaController.
```csharp
public ActionResult Index(Venta pVenta)
{
    if (pVenta == null)
    {
        pVenta = new Venta();
    }

    List<Venta> lista = ventaBL.Buscar(pVenta);

    // Cargar propiedades virtuales
    ventaBL.CargarClienteVirtual(lista);
    ventaBL.CargarEmpleadoVirtual(lista);

    // Listas de seleccion filtros
    ViewBag.Clientes = DropDownListCliente();
    ViewBag.Empleados = DropDownListEmpleado();

    return View(lista);
}
```

![image](https://github.com/user-attachments/assets/5bb8bd6c-f872-45d2-9c42-1abceac2199c)

**Paso 9:** Programar la accion **Details** de VentaController, servira para mostrar todos los detalles de una Venta.
```csharp
// GET: Venta/Details/5
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

**Paso 10:** Programar la accion **Create** de tipo **GET** en VentaController.

```csharp
// GET: Venta/Create
public ActionResult Create()
{
    // Listas de seleccion filtros
    ViewBag.Clientes = DropDownListCliente();
    ViewBag.Productos = ListaProductos();

    return View(new Venta());
}
```

![image](https://github.com/user-attachments/assets/7a416570-a4d6-4ff3-959d-05f60d150072)

**Paso 11:** Programar la accion **Create** de tipo **POST** en VentaController.

```csharp
[HttpPost]
public ActionResult Create(Venta pVenta)
{
    try
    {
        // TODO: Add insert logic here
        if (ModelState.IsValid)
        {
            // Obtener id del empleado logueado
            pVenta.IdEmpleado = ((Empleado)Session["usuario"]).IdEmpleado;
            int resultado = ventaBL.Guardar(pVenta);

            if (resultado > 0)
            {
                TempData["mensaje"] = "Registro guardado.";
                return RedirectToAction("Index");
            }
            else if (resultado == -1)
            {
                ModelState.AddModelError("", "Ya existe un registro con el mismo correlativo.");
            }
            else
            {
                ModelState.AddModelError("", "Error al registrar, intente de nuevo o contacte al soporte.");
            }
        }
    }
    catch (Exception ex)
    {
        ModelState.AddModelError("", ex.Message);
    }
    // Listas de seleccion filtros
    ViewBag.Clientes = DropDownListCliente();
    ViewBag.Productos = ListaProductos();

    return View(pVenta);
}
```

![image](https://github.com/user-attachments/assets/bf5abcbf-3433-4cae-95c7-fb0a5aece08e)

## PARTE 3 - Programación de la Vista Index

**Paso 1:** Colocar el mouse sobre la acción **Index**, luego dar clic derecho y seleccionar **Agregar Vista.**

**Paso 2:** Seleccionar **Vista de MVC 5** y dar clic en **Agregar**.

**Paso 3:** Seleccionar plantilla de tipo **"List"** y luego como clase modelo "Venta", y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/612e2f74-2bd6-4d5a-bec1-371c9b568b26)

**Resultado:**
![image](https://github.com/user-attachments/assets/ffa073e3-8efd-440a-ad8d-18af70970e93)

**Paso 4:** Actualizar los titulos y eliminar los enlaces.

![image](https://github.com/user-attachments/assets/3992794d-95e4-4039-9e77-399b29c359ea)

**Paso 5:** Actualizar la estructura de la tabla y configurar los datos de la tabla para mostrarlos ya tratados, y agregar el enlace **Ver Factura**.

![image](https://github.com/user-attachments/assets/e892b6ab-97ef-42e0-ab75-406852ebd8ef)

**Paso 6:** Agregar el html de los filtros de busqueda de la tabla Venta.
```razor
<form method="get" action="@Url.Action("Index","Venta")" class="row">
    <div class="col-12 col-lg-2 mt-2">
        <label class="form-label fw-bold" for="Correlativo">Correlativo:</label>
        <input type="text" class="form-control" name="Correlativo" placeholder="" />
    </div>

    <div class="col-12 col-lg-2 mt-2">
        <label class="form-label fw-bold" for="Correlativo">Fecha Hora:</label>
        @Html.TextBox("FechaHora", "", new { @type = "date", @class = "form-control datepicker", @Value = DateTime.Now.ToString("yyyy-MM-dd") })
    </div>

    <div class="col-12 col-lg-3 mt-2">
        <label class="form-label fw-bold" for="IdCargo">Empleado:</label>
        @Html.DropDownList("IdEmpleado", new SelectList(ViewBag.Empleados, "Value", "Text", null), new { @class = "form-select" })
    </div>

    <div class="col-12 col-lg-3 mt-2">
        <label class="form-label fw-bold" for="IdCargo">Cliente:</label>
        @Html.DropDownList("IdCliente", new SelectList(ViewBag.Clientes, "Value", "Text", null), new { @class = "form-select" })
    </div>

    <div class="col-12 col-lg-2 mt-2">
        <label class="form-label fw-bold">Estatus:</label>
        <select name="Estatus" class="form-select">
            <option value="0">Seleccionar</option>
            <option value="1">FACTURADA</option>
            <option value="2">ANULADA</option>
        </select>
    </div>

    <div class="col-12 col-lg-2 d-flex align-items-end mt-2">
        <button type="submit" class="btn btn-primary mx-2">BUSCAR</button>
        @Html.ActionLink("NUEVO", "Create", null, new { @Class = "btn btn-secondary" })
    </div>
</form>
```

![image](https://github.com/user-attachments/assets/4c31ee5a-6342-48b2-b688-7b6caa0c9be9)

**Paso 7:** Agregar la logica para mostrar alertas de resultado de los procesos de Venta.
```razor
@section scripts{

    @if (TempData["mensaje"] != null)
    {
        <script>
            Swal.fire({
                icon: "@(TempData["tipo"]!= null? TempData["tipo"]: "success")",
                title: "@TempData["mensaje"]",
                showConfirmButton: false,
                timer: 1500
            });
        </script>

        TempData["mensaje"] = null; // Vaciar memoria
    }
}
```

![image](https://github.com/user-attachments/assets/53462d8f-1ead-4df9-be1a-e08ceeba5b59)


**Paso 8:** Iniciar la aplicacion web en el IIS Express.

![image](https://github.com/user-attachments/assets/6eb19f1c-e99c-4215-bd75-479f6fd1307f)

**Paso 9:** Verificar que funcionen los filtros de la vista **Index** en **VentaController**.

![image](https://github.com/user-attachments/assets/cbe0c2f8-6920-4b7c-ba00-28d1e70e881a)

**Paso 10:** Detener la aplicacion web

![image](https://github.com/user-attachments/assets/cd85f0e0-8342-47b9-9426-ac8337b0cc59)

## PARTE 4 - Programación de la Vista Create

**Paso 1:** Colocar el mouse sobre la acción **Create**, luego dar clic derecho y seleccionar **Agregar Vista.**

**Paso 2:** Seleccionar **Vista de MVC 5** y dar clic en **Agregar**.

**Paso 3:** Seleccionar plantilla de tipo **"Create"** y luego como clase modelo "Venta", y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/0941787d-9886-472a-8d41-0e350151c4b6)

**Resultado:**
![image](https://github.com/user-attachments/assets/e47362cb-c71b-4ad1-a4a4-57f74193c04a)

**Paso 4:** Actualizar los titulos y eliminar los campos/elementos no necesarios del formulario.

![image](https://github.com/user-attachments/assets/ccb47cda-d0f1-4d5a-8e4f-2628b0910c37)

**Resultado:**

![image](https://github.com/user-attachments/assets/bf8ed950-d97a-468d-8f65-b483f66f8bfa)

**Paso 5:** Actualizar los estilos y configurar controles **FechaHora**, **Total** y **Botones**.

![image](https://github.com/user-attachments/assets/f5bffc52-16dd-4701-909f-64a32beac4ec)

**Resultado:**
![image](https://github.com/user-attachments/assets/476ba70d-9994-4ee8-83f7-969904e6a5b5)

**Resultado Grafico**
![image](https://github.com/user-attachments/assets/0910de4b-9a55-4222-a4ef-9fb51e07b7fb)

**Paso 6:** Agregar el sub formulario para agregar detalles a la venta.
```razor
    <fieldset class="border rounded p-2 mb-3">
        <legend>Detalles</legend>
        <div class="d-inline-flex">

            <select id="IdProducto" class="form-select" onchange="actualizarPrecio()">
                <option>Seleccionar</option>
                @foreach (var item in ViewBag.Productos as List<SistemaElParaisal.EN.Producto>)
                {
                    <option value="@item.IdProducto" data-precio="@item.Precio">@item.Nombre</option>
                }
            </select>

            <input type="number" id="Cantidad" class="form-control mx-2" placeholder="0" onchange="actualizarSubtotal()" />

            <input type="number" id="Precio" class="form-control" placeholder="$0.00" disabled />

            <input type="number" id="Subtotal" class="form-control mx-2" placeholder="$0.00" disabled />

            <button type="button" class="btn btn-secondary" onclick="agregarDetalle()">Agregar</button>
        </div>
    </fieldset>
```
![image](https://github.com/user-attachments/assets/c2ed5f62-bdd5-4a19-b7e1-1b72eb22984b)

**NOTAS:**
- 🟡 Los eventos HTML, **onchange** y **onclick** se agregaran despues en la programacion del codigo javascript.

**Paso 7:** Agregar la tabla para agregar detalles a la venta.
```razor
    <table class="table table-bordered table-striped">
        <thead>
            <tr>
                <th>Producto</th>
                <th>Cantidad</th>
                <th>Precio</th>
                <th>Subtotal</th>
                <th></th>
            </tr>
        </thead>
        <tbody id="tbodyDetalles"></tbody>
    </table>
```

![image](https://github.com/user-attachments/assets/bc29e48f-6991-4cb4-8b22-06b02189e4ab)

**Paso 8:** Agregar el javascript para actualizar automaticamente el precio y subtotal del detalle. 
- [querySelector()](https://www.w3schools.com/jsref/met_document_queryselector.asp) obtiene cualquier elemento del DOM mediante un selector de CSS.
```razor
@section scripts{
    <script>
        function numeroValido(valor) {
            if (!isNaN(parseFloat(valor)) && parseFloat(valor) > 0) {
                return true;
            }
            return false;
        }

        function actualizarPrecio() {
            // mostrar precio del producto seleccionado
            let producto = document.querySelector("#IdProducto > option:checked");
            let IdProducto = producto.value;

            let Precio = "";
            if (numeroValido(IdProducto)) {
                Precio = producto.getAttribute("data-Precio");
            }
            document.querySelector("#Precio").value = Precio;
            actualizarSubtotal();
        }

        function actualizarSubtotal() {
            // Actualizar SubTotal del detalle
            let Cantidad = document.querySelector("#Cantidad").value;
            let Precio = document.querySelector("#Precio").value;

            let Subtotal = "";
            if (numeroValido(Cantidad) && numeroValido(Precio)) {
                Subtotal = parseFloat(Cantidad) * parseFloat(Precio);
            }
            document.querySelector("#Subtotal").value = Subtotal;
        }
    </script>
}
```

![image](https://github.com/user-attachments/assets/c2b004d2-8816-49b7-ae95-ac080712995e)

**Resultado Grafico:**

![image](https://github.com/user-attachments/assets/70647bc3-340d-4324-a97f-9df85ccda460)

**Paso 9:** Agregar el javascript para agregar filas a la tabla de detalles de Venta.
```razor
function crearTD(name, index, value, show = true) {
    // Crear columna con datos serializados
    let html = "<td class=" + (show == false ? "d-none" : "") + ">";
    html += "       " + value + "";
    html += "       <input type='hidden' name='DetallesVenta[" + index + "]." + name + "' value='" + value + "' />";
    html += "   </td>";
    return html;
}

let _total = 0; // total de la venta
let _index = 0; // indice del array de DetallesVenta
function agregarFila(detalle) {

    let idTR = "trDetalle_" + detalle.IdProducto;
    var tbody = document.querySelector("#tbodyDetalles");

    let tr = "";
    tr += "<tr id='" + idTR + "' data-index='" + _index + "'>";
    tr += crearTD("IdProducto", _index, detalle.IdProducto, false);
    tr += crearTD("Producto.Nombre", _index, detalle.Producto.Nombre);
    tr += crearTD("Cantidad", _index, detalle.Cantidad);
    tr += crearTD("Precio", _index, detalle.Precio);
    tr += crearTD("Subtotal", _index, detalle.Subtotal);
    tr += "     <td>";
    tr += "         <button type='button' onclick='quitarDetalle(" + detalle.IdProducto + ")' class='btn btn-danger btn-sm'>x</button>";
    tr += "     </td>";
    tr += "</tr>";

    tbody.innerHTML += tr;
    _index++;
    // Actualizar Total
    _total += detalle.Subtotal;
    document.querySelector("#Total").value = _total;
}
```
![image](https://github.com/user-attachments/assets/b57c0db5-1adc-4ef9-bf93-354cadabfef2)

**NOTAS:**
- Los detalles se deben serializar para enviarlos a la lista de detalles en Venta.
- La serializacion consiste en enviar los datos en el formato y estructura correcta.
- Las listas de forma primitiva siempre seran un ARRAY, por ello el **indice** empieza en 0 y continua aumentando.
- Para enviar los datos serializados a un controlador, se deben enviar en un input con un name configurado segun la estructura de la propiedad a la que apuntamos.
- Para serializar una lista se deben enviar los datos de la siguiente forma:
    - DetallesVenta[0].IdProducto equivale al id del producto del detalle 1.
    - DetallesVenta[0].Cantidad equivale a la cantidad del producto del detalle 1.
    - DetallesVenta[0].Precio equivale al precio del producto del detalle 1.
    - DetallesVenta[0].Subtotal equivale al subtotal del producto del detalle 1.
- Para serializar un segundo detalle, debemos aumentar el numero del indice:
    - DetallesVenta[1].IdProducto equivale al id del producto del detalle 2.
    - DetallesVenta[1].Cantidad equivale a la cantidad del producto del detalle 2.
    - DetallesVenta[1].Precio equivale al precio del producto del detalle 2.
    - DetallesVenta[1].Subtotal equivale al subtotal del producto del detalle 2. 

 **Paso 10:** Agregar el javascript para agregar detalles si se completan los datos obligatorios.
```razor
function agregarDetalle() {
    // Capturar valores
    let Cantidad = document.querySelector("#Cantidad").value;
    let producto = document.querySelector("#IdProducto > option:checked");
    let IdProducto = producto.value;

    // Validaciones
    if (!numeroValido(IdProducto)) {
        return alert("Seleccione un producto");
    }

    let idTR = "trDetalle_" + IdProducto;
    if (document.querySelector("#" + idTR) != null) {
        return alert("El producto seleccionado, ya se encuentra en los detalles");
    }

    if (!numeroValido(Cantidad)) {
        return alert("Ingrese una Cantidad mayor a cero");
    }

    // Obtener datos del detalle
    let Nombre = producto.textContent; // Texto que muestra el select
    let Precio = producto.getAttribute("data-Precio");
    var Subtotal = parseFloat(Precio) * parseInt(Cantidad);

    // Crear objeto y agregarlo como fila
    let detalle = {
        IdProducto,
        Producto: { Nombre },
        Cantidad,
        Precio,
        Subtotal
    };
    agregarFila(detalle);
    // Resetear sub formulario
    document.querySelector("#IdProducto option").selected = true;
    document.querySelector("#Cantidad").value = "";
    document.querySelector("#IdProducto").onchange();
}
```

![image](https://github.com/user-attachments/assets/694142b9-ae52-4d0b-88bc-fb52a7b41f2f)


**Resultado Grafico:**

![image](https://github.com/user-attachments/assets/5cb90b52-4f36-449e-b76a-c19388f5eca5)

 **Paso 11:** Agregar el javascript para quitar detalles si es requerido y actualizar el indice del array.
```razor
function actualizarIndex() {
    _index = 0;
    _total = 0;
    let filas = document.querySelectorAll("#tbodyDetalles tr");
    for (var i = 0; i < filas.length; i++) {

        let indexTR = filas[i].getAttribute("data-index");
        filas[i].innerHTML = filas[i].innerHTML.replaceAll("[" + indexTR + "]", "[" + i + "]");

        filas[i].setAttribute("data-index", i);
        _index++;
        // Actualizar Total
        let subtotal = filas[i].querySelector("input[name='DetallesVenta[" + i + "].Subtotal']").value;
        _total += parseFloat(subtotal);
        document.querySelector("#Total").value = _total;
    }
}

function quitarDetalle(IdProducto) {
    let tr = document.querySelector("#trDetalle_" + IdProducto);
    tr.remove();
    actualizarIndex();
}
```

![image](https://github.com/user-attachments/assets/294194db-1b9a-4601-9dbb-839fc27fe443)

**Resultado Grafico:**

![image](https://github.com/user-attachments/assets/c103f551-6888-4b07-b512-84a952c0ad55)

**Resultado:**
![image](https://github.com/user-attachments/assets/f41800a4-0c60-4c48-a90a-f9597ae6a8a8)

 **Paso 12:** Agregar codigo razor para agregar los detalle de venta, si la pagina se recarga.
```razor
<!-- Permite agregar las filas si se recarga la vista -->
@if (Model.DetallesVenta != null && Model.DetallesVenta.Count > 0)
{
    foreach (var item in Model.DetallesVenta)
    {
        <script>
            // Agregar detalles convertidos en objetos
            agregarFila( @Html.Raw(Json.Encode(item)) );
        </script>
    }

}
```

![image](https://github.com/user-attachments/assets/b2adf26e-d080-4fb4-9182-f8ed3488adca)

**Paso 13:** Iniciar la aplicacion web en el IIS Express.

![image](https://github.com/user-attachments/assets/6eb19f1c-e99c-4215-bd75-479f6fd1307f)

**Paso 14:** Verificar que funcione el proceso de registro de la accion **Create** en **VentaController**.

![image](https://github.com/user-attachments/assets/47f80229-cd75-4be2-b03d-4b7ab5728e1f)

**Resultado:**

![image](https://github.com/user-attachments/assets/2f1b447f-fad1-4a57-bbbe-3ea274ff1315)

**Paso 15:** Detener la aplicacion web

![image](https://github.com/user-attachments/assets/cd85f0e0-8342-47b9-9426-ac8337b0cc59)
