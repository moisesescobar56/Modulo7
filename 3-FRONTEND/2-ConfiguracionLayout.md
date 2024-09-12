# CONFIGURACIÓN LAYOUT EN UI.APPWEBMVC

Para este proyecto se utilizaron librerias como:
- [Bootstrap 5 - v5.3.3](https://getbootstrap.com/docs/5.3/layout/grid/)
- [Font Awesome Free - v6.6.0](https://fontawesome.com/search?o=r&m=free)
- [Sweet alert 2 - v11.14.0](https://sweetalert2.github.io/)
- [jQuery - v3.7.1](https://jquery.com/download/)
- [jQuery Validation Plugin - v1.19.5](https://jqueryvalidation.org/documentation/)
- [SB Admin - v7.0.7](https://startbootstrap.com/template/sb-admin)

## PARTE 1 - Creacion y configuracion de carpetas
**Paso 1:** En el proyecto **"UI.AppWebMVC"**, seleccionar **Agregar > Nueva carpeta**, agregar las siguientes carpetas:
- Content
- Scripts

![image](https://github.com/user-attachments/assets/168a1f7c-089a-4183-b4e8-aa6106dd72eb)

**Resultado:**

![image](https://github.com/user-attachments/assets/147e2905-b280-4e9f-bbd2-532a88813bb9)

**Paso 2:** En la carpeta **Content**, seleccionar **Agregar > Nueva carpeta**, agregar las siguientes carpetas:
  - css
  - images
  - webfonts

![image](https://github.com/user-attachments/assets/80c3dba4-cb7a-4c46-be63-22ec2d4f0b36)

**Resultado:**

![image](https://github.com/user-attachments/assets/a07fbb8a-dc66-4995-8560-ea2e5e411806)

**Paso 3:** Descargar el archivo zip con las librerias [Descargar Aqui](https://github.com/user-attachments/files/16973713/LibreriasWeb.zip) y luego ir a la carpeta donde se guardo.

![image](https://github.com/user-attachments/assets/f9dde657-c84b-4e53-90b6-3674a865bc34)

**Resultado:**

![image](https://github.com/user-attachments/assets/953d5b41-fa45-4476-beb0-3da47050df1d)

**Paso 4:** Seleccionar el archivo zip y descomprimirlo con Winrar u otro programa.

![image](https://github.com/user-attachments/assets/bbbac5fc-c988-4c4b-a451-58f8e4fcb7ab)

**Resultado:**
![image](https://github.com/user-attachments/assets/aa6e6276-2593-473c-b30c-e621b59994b8)

**Paso 5:** Abrir la carpeta de Librerias.

![image](https://github.com/user-attachments/assets/509d31f6-11ca-47d6-ba24-226b126c2d1f)

**Paso 6:** Copiar las carpetas **Content** y **Scripts**, arrastrandolas y dejandolas caer sobre el proyecto web **"UI.AppWebMVC"**.

![image](https://github.com/user-attachments/assets/abb29a5b-9b03-4804-b88e-8c9228c757f8)

**Paso 7:** Seleccionar **Aplicar a todos los elementos** y dar clic en **SI**, si se muestra una advertencia.

![image](https://github.com/user-attachments/assets/4e901b4c-2fd0-4b00-b927-dbc10a3740d9)

**Resultado:**

![image](https://github.com/user-attachments/assets/6dfd065e-56d4-437d-8eb5-74ea111a1657)

## PARTE 2 - Creacion de Layout

**Paso 1:** En la carpeta **Views**, seleccionar **Agregar > Nueva carpeta**, y agregar la carpeta **Shared**:
```
Shared
```
![image](https://github.com/user-attachments/assets/f22bcb4a-5cf7-4ab3-90e1-f72128824e46)

**Paso 2:** En la carpeta **Shared**, seleccionar **Agregar > Pagina de diseño de MVC 5 (Razor)**.

![image](https://github.com/user-attachments/assets/53009580-594e-4b4c-b524-7df3613d3b6b)

**Resultado:**
![image](https://github.com/user-attachments/assets/c9506c5e-d91a-4fef-a2b9-cf003e89b7ea)

**Paso 3:** Renombrar el archivo como **_Layout** y dar clic en **Aceptar**.

![image](https://github.com/user-attachments/assets/6f0d4513-3f7c-4657-b23d-4deaffacc345)

**Resultado:**
![image](https://github.com/user-attachments/assets/73fa4923-e486-4b3a-8f38-d0582cc13f9b)

**Paso 4:** Borrar el contenido del **_Layout** y pegar la estructura HTML/Razor siguiente:
```razor
<!DOCTYPE html>

<html data-bs-theme="light">
<head>
    <meta charset="utf-8" />
    <title>@ViewBag.Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="icon" type="image/x-icon" href="~/Content/images/favicon.ico" />
    <!-- Styles -->
    <link href="~/Content/css/bootstrap.min.css" rel="stylesheet" />
    <!-- Icons -->
    <link href="~/Content/css/all.min.css" rel="stylesheet" />
    <script src="~/Scripts/all.min.js"></script>
    <!-- Sweet alert -->
    <link href="~/Content/css/sweetalert2.min.css" rel="stylesheet" />
    <!-- Layout -->
    <link href="~/Content/css/_layout.css" rel="stylesheet" />

    @RenderSection("styles", false) <!-- Insertar CSS desde una View -->
</head>
<body class="sb-nav-fixed">
    <!-- Logo and Burguer Button -->
    <nav class="sb-topnav navbar navbar-expand navbar-dark bg-dark fixed-top">
        <!-- Navbar Brand-->
        <a class="navbar-brand ps-3" href="#">
            <img src="~/Content/images/logo128x128.png" alt="logo" width="24" height="24" />
            El Paraisal
        </a>
        <!-- Sidebar Toggle-->
        <button type="button" class="btn btn-link btn-sm order-1 order-lg-0 me-4 me-lg-0" id="sidebarToggle" title="Toggle"><i class="fas fa-bars"></i></button>
        <!-- Navbar Search-->
        <form class="d-none d-md-inline-block form-inline ms-auto me-0 me-md-3 my-2 my-md-0">
        </form>
        <!-- Navbar-->
        <ul class="navbar-nav ms-auto ms-md-0 me-3 me-lg-4">
            <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" id="navbarDropdown" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false"><i class="fas fa-user fa-fw"></i></a>
                <ul class="dropdown-menu dropdown-menu-end" aria-labelledby="navbarDropdown">
                    <li>
                        <a class="dropdown-item" href="@Url.Action("View", "Controller")">
                            <i class="fa fa-gear"></i>  Configuración
                        </a>
                    </li>
                    <li><hr class="dropdown-divider" /></li>
                    <li>
                        <a class="dropdown-item" href="@Url.Action("View", "Controller")">
                            <i class="fa fa-right-to-bracket"></i>  Iniciar Sesión
                        </a>
                    </li>
                    <li>
                        <a class="dropdown-item" href="@Url.Action("View", "Controller")">
                            <i class="fa fa-power-off"></i> Cerrar Sesión
                        </a>
                    </li>
                </ul>
            </li>
        </ul>
    </nav>
    <!-- Lateral Nav -->
    <div id="layoutSidenav">
        <div id="layoutSidenav_nav">
            <nav class="sb-sidenav accordion sb-sidenav-dark" id="sidenavAccordion">
                <div class="sb-sidenav-menu">
                    <div class="nav">
                        <!-- Links -->
                        <div class="sb-sidenav-menu-heading">Mantenimiento</div>
                        <a class="nav-link" href="@Url.Action("Index", "Empleado")">
                            <div class="sb-nav-link-icon"><i class="fas fa-user"></i></div>
                            Empleados
                        </a>
                        <a class="nav-link" href="@Url.Action("Index", "Producto")">
                            <div class="sb-nav-link-icon"><i class="fas fa-box-open"></i></div>
                            Productos
                        </a>
                        <div class="sb-sidenav-menu-heading">Mantenimiento</div>
                        <!-- Link Collapse -->
                        <a class="nav-link collapsed" href="#" data-bs-toggle="collapse" data-bs-target="#collapseEmpleado" aria-expanded="false" aria-controls="collapseEmpleado">
                            <div class="sb-nav-link-icon"><i class="fas fa-user"></i></div>
                            Empleados
                            <div class="sb-sidenav-collapse-arrow"><i class="fas fa-angle-down"></i></div>
                        </a>
                        <div class="collapse" id="collapseEmpleado" aria-labelledby="headingOne" data-bs-parent="#sidenavAccordion">
                            <nav class="sb-sidenav-menu-nested nav">
                                <a class="nav-link" href="@Url.Action("Create", "Empleado")">Registrar</a>
                                <a class="nav-link" href="@Url.Action("Index", "Empleado")">Administrar</a>
                            </nav>
                        </div>
                        <!--Link Collapse -->
                        <a class="nav-link collapsed" href="#" data-bs-toggle="collapse" data-bs-target="#collapseProducto" aria-expanded="false" aria-controls="collapseProducto">
                            <div class="sb-nav-link-icon"><i class="fas fa-box-open"></i></div>
                            Productos
                            <div class="sb-sidenav-collapse-arrow"><i class="fas fa-angle-down"></i></div>
                        </a>
                        <div class="collapse" id="collapseProducto" aria-labelledby="headingOne" data-bs-parent="#sidenavAccordion">
                            <nav class="sb-sidenav-menu-nested nav">
                                <a class="nav-link" href="@Url.Action("Create", "Producto")">Registrar</a>
                                <a class="nav-link" href="@Url.Action("Index", "Producto")">Administrar</a>
                            </nav>
                        </div>
                    </div>
                </div>
                <div class="sb-sidenav-footer">
                    <div class="small">
                        <i class="fas fa-user-large"></i>
                        Usuario
                    </div>
                </div>
            </nav>
        </div>

        <div id="layoutSidenav_content">
            <main>
                <div class="container-fluid p-3">
                    <!-- Content -->
                    @RenderBody()
                </div>
            </main>
            <footer class="py-4 bg-light mt-auto">
                <div class="container-fluid px-4">
                    <div class="d-flex align-items-center justify-content-between small">
                        <!-- Footer -->
                        <div class="text-muted">Copyright &copy; El Paraisal @DateTime.Now.Year</div>
                    </div>
                </div>
            </footer>
        </div>
    </div>
    <script src="~/Scripts/bootstrap.bundle.min.js"></script>
    <script src="~/Scripts/jquery.min.js"></script>
    <script src="~/Scripts/jquery.validate.min.js"></script>
    <script src="~/Scripts/sweetalert2.all.min.js"></script>
    <script src="~/Scripts/_layout.js"></script>
    <script src="~/Scripts/utilidades.js"></script>

    @RenderSection("scripts", false) <!-- Insertar JS desde una View -->
</body>
</html>
```

**Resultado:**
![image](https://github.com/user-attachments/assets/42153ce5-5090-4dfc-84ac-e23ec8b742b1)

# PARTE 3 - Crear HomeController e Index de la aplicacion web

**Paso 1:** En la carpeta **Controllers**, seleccionar **Agregar > Controlador**.

![image](https://github.com/user-attachments/assets/e10e5ab8-cb80-40a0-959a-21fa3f586ad2)

**Paso 2:** Seleccionar **Controlador de MVC 5: en blanco** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/0990668b-80b4-4b3e-bf9d-0567bfbed77e)

**Resultado:**
![image](https://github.com/user-attachments/assets/d2515076-36ab-4019-82d9-c2501cdbe791)

**Paso 3:** Renombrar el controlador como **HomeController** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/d233875a-d579-4e0b-8a81-db855db6d25c)

**Resultado:**
![image](https://github.com/user-attachments/assets/0385c815-0a37-45c3-8688-bdf6eb9ec34e)

**Paso 4:** Dar clic derecho sobre la accion **Index** y seleccionar **Agregar Vista**.

![image](https://github.com/user-attachments/assets/cc0b2d22-d3cb-49d4-9272-ff39f9bdeb02)

**Resultado:**
![image](https://github.com/user-attachments/assets/752b0d4d-ad97-4549-9996-bb3083a83e3e)

**Paso 5:** Seleccionar una **Vista de MVC 5** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/8758f831-93e9-476a-8b96-5b741b3e8632)

**Paso 6:** Marcar la opcion **"Usar pagina de diseño"** y dar clic en el boton de examinar **"..."**

![image](https://github.com/user-attachments/assets/d837deb7-1f38-46cd-802a-5a18a711f7bc)

**Paso 7:** Seleccionar la pagina de diseño **"_Layout.cshtml"** y dar clic en **Aceptar**.

![image](https://github.com/user-attachments/assets/3ae9f931-5294-48f6-9f72-67f8f4ebe222)

**Paso 8:** Verificar que se haya seleccionar la pagina de diseño **"_Layout.cshtml"** y dar clic en **Agregar**.

![image](https://github.com/user-attachments/assets/134d6464-b99b-48ad-8c93-3a80af20cab7)

**Resultado:**
![image](https://github.com/user-attachments/assets/10ce3b24-2945-4c87-9f8c-a0d1621db126)

**Paso 9:** Iniciar el servidor IIS. 

![image](https://github.com/user-attachments/assets/6eb19f1c-e99c-4215-bd75-479f6fd1307f)

**Resultado:**
![image](https://github.com/user-attachments/assets/7386027d-f495-4b97-9e5a-86d0a513f649)

**Paso 10:** Detener el servidor IIS.
![image](https://github.com/user-attachments/assets/cd85f0e0-8342-47b9-9426-ac8337b0cc59)
