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


**Paso 8:** Pogramar la accion **Index** de VentaController.
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

**Paso 9:** Pogramar la accion **Details** de VentaController, servira para mostrar todos los detalles de una Venta.
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

