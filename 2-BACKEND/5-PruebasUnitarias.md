# AUTOMATIZACIÓN DE PRUEBAS UNITARIAS

## ¿Que es una prueba unitaria?
Las pruebas unitarias son el proceso en el que se prueba la unidad funcional de código más pequeña. 

## ¿Por qué son importantes?
Las pruebas de software ayudan a garantizar la calidad del código y son una parte integral del desarrollo de software. Una práctica recomendada en el desarrollo de software es escribir el software como unidades pequeñas y funcionales, y luego escribir una prueba unitaria para cada unidad de código. 

## ¿Comó automatizar las pruebas unitarias ?
Puede escribir primero pruebas unitarias como código. Luego, ejecute ese código de prueba de forma automática cada vez que realice cambios en el código del software. De esta forma, si una prueba falla, puede aislar con rapidez el área del código que tiene el error. 

![image](https://github.com/user-attachments/assets/bdc1aa54-b9f4-4fba-bdf6-bd9d19d1dffc)

## PARTE 1 - Configuración de archivo “CargoUnitTest.cs”

**Paso 1:** Seleccionar el proyecto **“SistemaElParaisal.PruebasUnitarias”**, dar clic derecho y seleccionar 
**Agregar > Prueba Unitaria**.

![image](https://github.com/user-attachments/assets/cebf6194-4392-4147-8099-50b20d0a087f)

**Paso 2:** Dar clic derecho sobre el archivo **UnitTest1.cs** y seleccionar **Cambiar nombre**.

![image](https://github.com/user-attachments/assets/68901003-53b5-4258-a0fb-09ca8e981ee4)

**Paso 3:** Ingresar el nombre **“CargoUnitTest.cs”** y pulsar la tecla Enter. 

![image](https://github.com/user-attachments/assets/8d9d3a94-ed76-4de9-9a8d-75f4189880b4)

**Paso 4:** En el siguiente cuadro de dialogo seleccionar **"Si"**. Para que cambie en todas las referencias el 
nombre del archivo. 

![image](https://github.com/user-attachments/assets/2e1dd1cd-92c6-4275-a3d3-17f1b81efdf6)

**Resultado:**

![image](https://github.com/user-attachments/assets/38793d30-0ce3-4adf-92f5-74504b4bd563)

**Paso 5:** Agregar las **referencias a bibliotecas** para acceder a la tabla Cargo en la base de datos 
desde el archivo **“CargoUnitTest.cs”**.

```csharp
// Referencias
using System.Collections.Generic;
// Referencias del proyecto
using SistemaElParaisal.EN;
using SistemaElParaisal.BL;
```

![image](https://github.com/user-attachments/assets/49328cc5-898b-4ca5-b71d-7443868ec14b)

**Paso 6:** Borrar el método **TestMethod1** del archivo **“CargoUnitTest.cs”**.

![image](https://github.com/user-attachments/assets/e58392e5-6bfc-4de5-8ba6-23a2e725a734)

**Resultado:**
![image](https://github.com/user-attachments/assets/7e28b57f-79d8-4042-be64-7df685f069bb)

## PARTE 2 - Programación de métodos de prueba en “CargoUnitTest.cs” 

![image](https://github.com/user-attachments/assets/9d90a250-ee2c-48cf-9d07-900131de0dab)

**Paso 1:** Agregar la instancia a la clase **CargoBL** para acceder a la tabla Cargos de la base de  datos.
```csharp
// Conexion a la tabla de Cargos en la DB mediante una instancia de CargoBL
CargoBL cargoBL = new CargoBL();
```
![image](https://github.com/user-attachments/assets/a2374c10-07b6-43e4-84d7-51c2000382f9)

**Paso 2:** Programar el **[TestMethod]** para comprobar el funcionamiento del método **ObtenerPorId**
de **CargoBL**. 

```csharp
[TestMethod]
public void T1_ObtenerPorId()
{
    // Buscar el cargo con id = 1
    Cargo cargo = cargoBL.ObtenerPorId(1);

    // Comprobacion de la prueba
    // si el Id de Cargo es diferente de 0, el Cargo se obtuvo por Id correctamente
    Assert.AreNotEqual(0, cargo.IdCargo);
}
```
![image](https://github.com/user-attachments/assets/3319ccab-d58c-403a-8907-81183120b833)

**NOTA:** La configuracion del nombre de las pruebas unitarias es conformado por la siguiente estructura: 

**"T"** + ***"Numero de prueba"*** + **"_"** + ***"Metodo a probar"*** 

**Paso 3:** Programar el **[TestMethod]** para comprobar el funcionamiento del método **Buscar** de 
**CargoBL**.
```csharp
[TestMethod]
public void T2_Buscar()
{
    List<Cargo> lista = cargoBL.Buscar(new Cargo { Nombre = "ADMIN" });

    // Comprobacion de la prueba
    // si el total de la lista es diferente de 0, los Cargos se buscaron correctamente
    Assert.AreNotEqual(0, lista.Count);
}
```

![image](https://github.com/user-attachments/assets/61f52df4-3742-4331-a739-de1bd5136c8e)


## PARTE 3 - Configuración de archivo “EmpleadoUnitTest.cs”

**Paso 1:** Seleccionar el proyecto **“SistemaElParaisal.PruebasUnitarias”**, dar clic derecho y seleccionar 
**Agregar > Prueba Unitaria**.

![image](https://github.com/user-attachments/assets/cebf6194-4392-4147-8099-50b20d0a087f)

**Paso 2:** Dar clic derecho sobre el archivo **UnitTest1.cs** y seleccionar **Cambiar nombre**.

![image](https://github.com/user-attachments/assets/1a579eef-9773-44c0-a23a-0796f2239206)


**Paso 3:** Ingresar el nombre **“EmpleadoUnitTest.cs”** y pulsar la tecla Enter. 

![image](https://github.com/user-attachments/assets/23eaa2da-5ddc-4427-893d-b3370f8b4719)

**Paso 4:** En el siguiente cuadro de dialogo seleccionar **"Si"**. Para que cambie en todas las referencias el 
nombre del archivo. 

![image](https://github.com/user-attachments/assets/be69a2ac-e0cb-4694-93df-4cb4a4923262)

**Resultado:**

![image](https://github.com/user-attachments/assets/c167e46a-04c5-4660-9fa1-8afaac4a3da0)


**Paso 5:** Agregar las **referencias a bibliotecas** para acceder a la tabla Empleado en la base de datos 
desde el archivo “EmpleadoUnitTest.cs”.

```csharp
// Referencias
using System.Collections.Generic;
// Referencias del proyecto
using SistemaElParaisal.EN;
using SistemaElParaisal.BL;
```

![image](https://github.com/user-attachments/assets/d0aeb952-13ee-4ef3-88ae-797665698028)

**Paso 6:** Borrar el método TestMethod1 del archivo **“CargoUnitTest.cs”**.

![image](https://github.com/user-attachments/assets/87a0f018-8f64-4aee-a08e-04c288c986f1)

**Resultado:**
![image](https://github.com/user-attachments/assets/097406ec-4783-4a83-9d73-4d6371a41455)

## PARTE 4 - Programación de métodos de prueba en “EmpleadoUnitTest.cs” 

![image](https://github.com/user-attachments/assets/9d90a250-ee2c-48cf-9d07-900131de0dab)

**Paso 1:** Agregar la instancia a la clase **EmpleadoBL** para acceder a la tabla Empleados de la base de  datos.
```csharp
// Conexion a la tabla de Empleados en la DB mediante una instancia de EmpleadoBL
EmpleadoBL empleadoBL = new EmpleadoBL();
```
![image](https://github.com/user-attachments/assets/6a6e4945-0135-4470-9125-4e0b416789de)

**Paso 2:** Programar el **[TestMethod]** para comprobar el funcionamiento del método **Guardar**
de EmpleadoBL. 
```csharp
[TestMethod]
public void T1_Guardar()
{
    // instancia de Empleado con los datos a guardar
    Empleado empleado = new Empleado();
    empleado.IdCargo = 1;
    empleado.Nombre = "Ramon";
    empleado.Apellido = "Perez";
    empleado.Telefono = "12341234";
    empleado.Clave = "123";
    // Ejecutar metodo 
    int resultado = empleadoBL.Guardar(empleado);

    // Comprobacion de la prueba
    // si resultado es igual a 1, la Empleado se guardo correctamente
    Assert.AreEqual(1, resultado);
}
```

![image](https://github.com/user-attachments/assets/841adca7-1842-43e8-96f0-5a15be922aa4)

**Paso 3:** Programar el **[TestMethod]** para comprobar el funcionamiento del método **Modificar**
de EmpleadoBL. 
```csharp
[TestMethod]
public void T2_Modificar()
{
    // instancia de Empleado con los datos a modificar
    Empleado empleado = empleadoBL.ObtenerPorId(1);
    empleado.IdCargo = 1;
    empleado.Nombre = "Juan";
    empleado.Apellido = "Perez";
    empleado.Telefono = "98001234";
    empleado.Clave = "123";
    // Ejecutar metodo
    int resultado = empleadoBL.Modificar(empleado);

    // Comprobacion de la prueba
    // si resultado es igual a 1, la Empleado se modifico correctamente
    Assert.AreEqual(1, resultado);
}
```

![image](https://github.com/user-attachments/assets/ec598dfd-cfb1-42b6-b92f-42e189dd9225)


**Paso 4:** Programar el **[TestMethod]** para comprobar el funcionamiento del método **Eliminar**
de EmpleadoBL. 
```csharp
[TestMethod]
public void T3_Eliminar()
{
    // Buscar el empleado con id = 3
    Empleado empleado = empleadoBL.ObtenerPorId(3);
    // Ejecutar metodo		
    int resultado = empleadoBL.Eliminar(empleado);

    // Comprobacion de la prueba
    // si resultado es igual a 1, la Empleado se elimino correctamente
    Assert.AreEqual(1, 1);
}
```

![image](https://github.com/user-attachments/assets/315581a2-d048-4285-87d8-64e7bf34229c)

**Paso 5:** Programar el **[TestMethod]** para comprobar el funcionamiento del método **ObtenerPorId**
de EmpleadoBL. 
```csharp
[TestMethod]
public void T4_ObtenerPorId()
{
    // Buscar el empleado con id = 1
    Empleado empleado = empleadoBL.ObtenerPorId(1);

    // Comprobacion de la prueba
    // si el Id de Empleado es diferente de 0, la Empleado se obtuvo por Id correctamente
    Assert.AreNotEqual(0, empleado.IdEmpleado);
}
```

![image](https://github.com/user-attachments/assets/5f073794-914a-47ba-b855-3c7484790353)


**Paso 6:** Programar el **[TestMethod]** para comprobar el funcionamiento del método **Buscar**
de EmpleadoBL. 
```csharp
[TestMethod]
public void T5_Buscar()
{
    List<Empleado> lista = empleadoBL.Buscar(new Empleado { Nombre = "Juan" });

    // Comprobacion de la prueba
    // si el total de la lista es diferente de 0, las Empleado se buscaron correctamente
    Assert.AreNotEqual(0, lista.Count);
}
```

![image](https://github.com/user-attachments/assets/6ca6e2fc-997f-4034-9c88-ca7bf942f90b)


## PARTE 5 - Ejecutar Pruebas Unitarias desde Visual Studio 
**Paso 1:** Seleccionar el proyecto “SistemaElParaisal.PruebasUnitarias” y dar clic derecho sobre el 
proyecto y seleccionar **Ejecutar pruebas**.

![image](https://github.com/user-attachments/assets/5ed370b9-4071-4043-aaa7-f5f015bb6bda)

**Resultado:**
![image](https://github.com/user-attachments/assets/e939c43c-c8ec-4939-ae2d-9b8369ed8867)


## Archivo **CargoUnitTest.cs**
```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System;
// Referencias
using System.Collections.Generic;
// Referencias de proyecto
using SistemaElParaisal.EN;
using SistemaElParaisal.BL;

namespace SistemaElParaisal.PruebasUnitarias
{
    [TestClass]
    public class CargoUnitTest
    {
        // Conexion a la tabla de Cargos en la DB mediante una instancia de CargoBL
        CargoBL cargoBL = new CargoBL();

        [TestMethod]
        public void T1_ObtenerPorId()
        {
            // Buscar el cargo con id = 1
            Cargo cargo = cargoBL.ObtenerPorId(1);

            // Comprobacion de la prueba
            // si el Id de Cargo es diferente de 0, el Cargo se obtuvo por Id correctamente
            Assert.AreNotEqual(0, cargo.IdCargo);
        }

        [TestMethod]
        public void T2_Buscar()
        {
            List<Cargo> lista = cargoBL.Buscar(new Cargo { Nombre = "ADMIN" });

            // Comprobacion de la prueba
            // si el total de la lista es diferente de 0, los Cargos se buscaron correctamente
            Assert.AreNotEqual(0, lista.Count);
        }
    }
}
```

## Archivo **EmpleadoUnitTest.cs**
```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System;
// Referencias
using System.Collections.Generic;
// Referencias del proyecto
using SistemaElParaisal.EN;
using SistemaElParaisal.BL;

namespace SistemaElParaisal.PruebasUnitarias
{
    [TestClass]
    public class EmpleadoUnitTest
    {
        // Conexion a la tabla de Empleados en la DB mediante una instancia de EmpleadoBL
        EmpleadoBL empleadoBL = new EmpleadoBL();

        [TestMethod]
        public void T1_Guardar()
        {
            // instancia de Empleado con los datos a guardar
            Empleado empleado = new Empleado();
            empleado.IdCargo = 1;
            empleado.Nombre = "Ramon";
            empleado.Apellido = "Perez";
            empleado.Telefono = "12341234";
            empleado.Clave = "123";
            // Ejecutar metodo 
            int resultado = empleadoBL.Guardar(empleado);

            // Comprobacion de la prueba
            // si resultado es igual a 1, la Empleado se guardo correctamente
            Assert.AreEqual(1, resultado);
        }

        [TestMethod]
        public void T2_Modificar()
        {
            // instancia de Empleado con los datos a modificar
            Empleado empleado = empleadoBL.ObtenerPorId(1);
            empleado.IdCargo = 1;
            empleado.Nombre = "Juan";
            empleado.Apellido = "Perez";
            empleado.Telefono = "98001234";
            empleado.Clave = "123";
            // Ejecutar metodo
            int resultado = empleadoBL.Modificar(empleado);

            // Comprobacion de la prueba
            // si resultado es igual a 1, la Empleado se modifico correctamente
            Assert.AreEqual(1, resultado);
        }

        [TestMethod]
        public void T3_Eliminar()
        {
            // Buscar el empleado con id = 3
            Empleado empleado = empleadoBL.ObtenerPorId(3);
            // Ejecutar metodo		
            int resultado = empleadoBL.Eliminar(empleado);

            // Comprobacion de la prueba
            // si resultado es igual a 1, la Empleado se elimino correctamente
            Assert.AreEqual(1, 1);
        }

        [TestMethod]
        public void T4_ObtenerPorId()
        {
            // Buscar el empleado con id = 1
            Empleado empleado = empleadoBL.ObtenerPorId(1);

            // Comprobacion de la prueba
            // si el Id de Empleado es diferente de 0, la Empleado se obtuvo por Id correctamente
            Assert.AreNotEqual(0, empleado.IdEmpleado);
        }

        [TestMethod]
        public void T5_Buscar()
        {
            List<Empleado> lista = empleadoBL.Buscar(new Empleado { Nombre = "Juan" });

            // Comprobacion de la prueba
            // si el total de la lista es diferente de 0, las Empleado se buscaron correctamente
            Assert.AreNotEqual(0, lista.Count);
        }
    }
}
```
## Archivo **Script_ElParaisalDB.sql**
```sql
-- MODO DESARROLLADOR
USE master;
GO
ALTER DATABASE ElParaisalDB SET SINGLE_USER WITH ROLLBACK IMMEDIATE;
GO
DROP DATABASE ElParaisalDB;
GO
-- FIN MODO DESARROLLADOR

----FASE 1 Y 2: Crear base de datos
-- Crear base de datos: Auth de Authentication/Autenticacion
CREATE DATABASE ElParaisalDB;
GO 
--Seleccionar BD
USE ElParaisalDB;
GO 
--Crear tablas independientes sin llave foranea
CREATE TABLE Cargo(
	IdCargo TINYINT IDENTITY PRIMARY KEY,
	Nombre VARCHAR(20) NOT NULL UNIQUE
);
GO
CREATE TABLE Categoria(
	IdCategoria TINYINT IDENTITY PRIMARY KEY,
	Nombre VARCHAR(20) NOT NULL UNIQUE
);
GO
CREATE TABLE Cliente(
	IdCliente INT PRIMARY KEY IDENTITY,
	Nombre VARCHAR(50) NOT NULL,
	Apellido VARCHAR(50) NOT NULL,
	DUI CHAR(9) NOT NULL UNIQUE,
	Telefono CHAR(8) NOT NULL,
	CorreoElectronico VARCHAR(120) NOT NULL,
	Direccion VARCHAR(120) NOT NULL
)
GO
--Crear tablas dependientes con llave foranea
CREATE TABLE Empleado(
	IdEmpleado SMALLINT PRIMARY KEY IDENTITY,
	IdCargo TINYINT NOT NULL REFERENCES Cargo,
	Nombre VARCHAR(50) NOT NULL,
	Apellido VARCHAR(50) NOT NULL,
	Telefono CHAR(8) NOT NULL UNIQUE,
	Clave VARCHAR(250) NOT NULL
)
GO
CREATE TABLE Producto(
	IdProducto INT IDENTITY PRIMARY KEY,
	IdCategoria TINYINT NOT NULL REFERENCES Categoria,
	Nombre VARCHAR(50) NOT NULL UNIQUE,
	Precio DECIMAL(5,2) NOT NULL
);
GO
CREATE TABLE Venta(
	IdVenta BIGINT PRIMARY KEY IDENTITY,
	IdEmpleado SMALLINT NOT NULL REFERENCES Empleado,
	IdCliente INT NOT NULL REFERENCES Cliente,
	Correlativo BIGINT NOT NULL UNIQUE,
	FechaHora DATETIME NOT NULL,
	Total DECIMAL(9,2) NOT NULL,
	Estatus TINYINT NOT NULL
)
GO
CREATE TABLE DetalleVenta(
	IdDetalleVenta BIGINT PRIMARY KEY IDENTITY,
	IdVenta BIGINT NOT NULL REFERENCES Venta,
	IdProducto INT NOT NULL REFERENCES Producto,
	Precio DECIMAL(5,2) NOT NULL,
	Cantidad INT NOT NULL,
	Total DECIMAL(9,2) NOT NULL
)
GO
-- FASE 3: Agregar registros
INSERT INTO Cliente (Nombre, Apellido, DUI, Telefono, CorreoElectronico, Direccion)
VALUES ('JUAN', 'PEREZ', '123456789', '67345678', 'jp@gmail.com','Sonsonate, Col. Las Brisas, Psj #1, Casa #10'),
	   ('LUIS', 'SANCHEZ', '123456781', '78456789', 'ls@gmail.com','San Antonio, Col. Primavera, Psj #3 Block A, Casa #4'),
	   ('CAMILA', 'JUAREZ', '123456782', '72456789', 'cj@gmail.com','Caluco, Canton Plan de Amayo, Psj #1, Casa #20'),
	   ('ROMINA', 'MARTINEZ', '123456783', '74456789', 'cj@gmail.com','Sonzacate, R. Loma Alta, Senda #1, Casa #5');
GO
SELECT IdCliente, Nombre, Apellido, DUI, Telefono, CorreoElectronico, Direccion
FROM Cliente;
GO
INSERT INTO Cargo (Nombre) 
VALUES ('Administrador'), 
	   ('Vendedor');
GO
SELECT IdCargo, Nombre
FROM Cargo;
GO
INSERT INTO Empleado (IdCargo, Nombre, Apellido, Telefono, Clave)
VALUES (1, 'Juan', 'Perez', '72001564', 'a665a45920422f9d417e4867efdc4fb8a04a1f3fff1fa07e998e86f7f7a27ae3'), -- 123
	   (2, 'Carmen', 'Perez', '65401115', 'a665a45920422f9d417e4867efdc4fb8a04a1f3fff1fa07e998e86f7f7a27ae3'), -- 123
	   (2, 'Ricardo', 'Lopez', '78121815', 'a665a45920422f9d417e4867efdc4fb8a04a1f3fff1fa07e998e86f7f7a27ae3'); -- 123
GO
SELECT IdEmpleado, IdCargo, Nombre, Apellido, Telefono, Clave
FROM Empleado;
GO
INSERT INTO Categoria (Nombre)
VALUES ('BEBIDAS FRIAS'),
	   ('BEBIDAS CALIENTES'),
	   ('DESAYUNOS'),
	   ('ALMUERZOS');
GO
SELECT IdCategoria, Nombre
FROM Categoria;
GO
INSERT INTO Producto (IdCategoria, Nombre, Precio)
VALUES (1, 'Coca-Cola Lata', 0.50),
		(1, 'Pepsi Lata', 0.50),
		(2, 'Café americano', 0.50),
		(3, 'Pancakes', 2.50),
		(3, 'Desayuno Tipico', 3.50),
		(4, 'Churrasco Argentino', 3.50),
		(4, 'Lasaña de Carne', 3.50);
GO
SELECT IdProducto, IdCategoria, Nombre, Precio
FROM Producto;
GO
-- Registro de Ventas
-- ESTATUS: 1 = FACTURADA y 2 = ANULADA
INSERT INTO Venta (IdEmpleado, IdCliente, Correlativo, FechaHora, Total, Estatus)
VALUES (2, 1, 120, '2024-01-05 15:00', 4, 2),
		(2,2, 121, GETDATE(), 4, 1);
GO
SELECT * 
FROM Venta;
GO
-- Registro de DetalleVenta
INSERT INTO DetalleVenta (IdVenta, IdProducto, Precio, Cantidad, Total)
VALUES (1 , 1, 0.5, 1, 0.5),
	   (1 , 7, 3.5, 1, 3.5),
	   (2 , 2, 0.5, 1, 0.5),
	   (2 , 6, 3.5, 1, 3.5);
GO
SELECT * 
FROM DetalleVenta;
GO
```

## Archivo **Script_ProcedimientosAlmacenados.sql**
```sql
USE ElParaisalDB;
GO
/**************  PROCEDIMIENTOS ALMACENADOS  **************/

/* TABLA CARGO */
-- Obtener Cargo por ID
CREATE PROCEDURE SP_ObtenerCargoPorId
	@IdCargo TINYINT
AS
BEGIN
	SELECT TOP 1 IdCargo, Nombre 
	FROM Cargo
	WHERE IdCargo = @IdCargo
END
GO
-- Test Obtener Cargo por ID
EXEC SP_ObtenerCargoPorId @IdCargo = 2
GO 

/* TABLA EMPLEADO */
-- Insertar Empleado
CREATE PROCEDURE SP_InsertarEmpleado
	@IdCargo TINYINT,
    @Nombre VARCHAR(50),
    @Apellido VARCHAR(50),
    @Telefono NCHAR(8),
    @Clave VARCHAR(250)
AS
BEGIN
	IF EXISTS (SELECT * FROM Empleado WHERE Telefono = @Telefono)
		BEGIN 
			PRINT 'Empleado ya registrado, numero de telefono ya ocupado'
		END
	ELSE
		BEGIN
			INSERT INTO Empleado (IdCargo, Nombre, Apellido, Telefono, Clave)
			VALUES (@IdCargo, @Nombre, @Apellido, @Telefono, @Clave)
		END
END
GO
-- Test de Insertar Empleado
EXEC SP_InsertarEmpleado @IdCargo = 1, @Nombre='Raul',@Apellido='Lue', @Telefono='72001565', @Clave='a665a45920422f9d417e4867efdc4fb8a04a1f3fff1fa07e998e86f7f7a27ae3'
GO
-- Modificar Empleado
CREATE PROCEDURE SP_ModificarEmpleado
    @IdEmpleado SMALLINT,
    @IdCargo TINYINT,
    @Nombre VARCHAR(50),
    @Apellido VARCHAR(50),
    @Telefono NCHAR(8),
    @Clave VARCHAR(250)
AS
BEGIN
    IF EXISTS (SELECT * FROM Empleado WHERE Telefono = @Telefono AND IdEmpleado != @IdEmpleado)
		BEGIN
			PRINT 'Empleado ya registrado';
		END
    ELSE
		BEGIN
			UPDATE Empleado
			SET IdCargo = @IdCargo, Nombre = @Nombre, Apellido = @Apellido, Telefono = @Telefono, Clave = @Clave
			WHERE IdEmpleado = @IdEmpleado;
			PRINT 'Empleado modificado correctamente';
		END
END;
GO
-- Test de Modificar Empleado
EXEC SP_ModificarEmpleado @IdEmpleado=1, @IdCargo = 1, @Nombre='Juan Antonio', @Apellido='Perez Rodriguez', @Telefono='72001565', @Clave='a665a45920422f9d417e4867efdc4fb8a04a1f3fff1fa07e998e86f7f7a27ae3'
GO
-- Eliminar Empleado
CREATE PROCEDURE SP_EliminarEmpleado
    @IdEmpleado SMALLINT
AS
BEGIN
    IF EXISTS (SELECT * FROM Venta WHERE IdEmpleado = @IdEmpleado)
		BEGIN
			PRINT 'Empleado asociado a registros de ventas, no puede darse de baja';
		END
    ELSE
		BEGIN
			DELETE FROM Empleado
			WHERE IdEmpleado = @IdEmpleado;
			PRINT 'Empleado eliminado correctamente';
		END
END;
GO
-- Test de Eliminar Empleado
EXEC SP_EliminarEmpleado @IdEmpleado = 4;
GO
-- Obtener Empleado por ID
CREATE PROCEDURE SP_ObtenerEmpleadoPorId
	@IdEmpleado SMALLINT
AS
BEGIN
    SELECT TOP 1 IdEmpleado, IdCargo, Nombre, Apellido, Telefono
    FROM Empleado
	WHERE IdEmpleado = @IdEmpleado;
END;
GO
-- TestO btener Empleado por ID
EXEC SP_ObtenerEmpleadoPorId @IdEmpleado = 1
GO
/* TABLA CLIENTE */
-- Insertar Cliente
CREATE PROCEDURE SP_InsertarCliente
    @Nombre VARCHAR(50),
    @Apellido VARCHAR(50),
    @DUI NCHAR(9),
    @Telefono NCHAR(8),
	@CorreoElectronico VARCHAR(120),
    @Direccion VARCHAR(200)
AS
BEGIN
	IF EXISTS (SELECT * FROM Cliente WHERE DUI = @DUI)
		BEGIN 
			PRINT 'Cliente ya registrado, numero de DUI ya ocupado'
		END
	ELSE
		BEGIN
			INSERT INTO Cliente (Nombre, Apellido, DUI, Telefono, CorreoElectronico, Direccion)
			VALUES (@Nombre, @Apellido, @DUI, @Telefono, @CorreoElectronico, @Direccion)
		END
END
GO
-- Test de Insertar Cliente
EXEC SP_InsertarCliente @Nombre='Cristian',@Apellido='Gil', @DUI='123456784', @Telefono='77601578', @CorreoElectronico='cg@gmail.com', @Direccion='Izalco, Col. El Prado, Psj #1, Casa #12'
GO
-- Modificar Cliente
CREATE PROCEDURE SP_ModificarCliente
    @IdCliente INT,
    @Nombre VARCHAR(50),
    @Apellido VARCHAR(50),
    @DUI NCHAR(9),
    @Telefono NCHAR(8),
	@CorreoElectronico VARCHAR(120),
    @Direccion VARCHAR(200)
AS
BEGIN
    IF EXISTS (SELECT * FROM Cliente WHERE DUI = @DUI AND IdCliente != @IdCliente)
		BEGIN
			PRINT 'Cliente ya registrado';
		END
    ELSE
		BEGIN
			UPDATE Cliente
			SET Nombre = @Nombre, Apellido = @Apellido, DUI = @DUI, Telefono = @Telefono, CorreoElectronico = @CorreoElectronico, Direccion = @Direccion
			WHERE IdCliente = @IdCliente;
			PRINT 'Cliente modificado correctamente';
		END
END;
GO
-- Test de Modificar Cliente
EXEC SP_ModificarCliente @IdCliente=5, @Nombre='Cristian',@Apellido='Gil', @DUI='123456784', @Telefono='77601578', @CorreoElectronico='cg@gmail.com', @Direccion='Izalco, Col. El Prado, Psj #1, Casa #12'
GO
-- Eliminar Cliente
CREATE PROCEDURE SP_EliminarCliente
    @IdCliente INT
AS
BEGIN
    IF EXISTS (SELECT * FROM Venta WHERE IdCliente = @IdCliente)
		BEGIN
			PRINT 'Cliente asociado a registros de ventas, no puede darse de baja';
		END
    ELSE
		BEGIN
			DELETE FROM Cliente
			WHERE IdCliente = @IdCliente;
			PRINT 'Cliente eliminado correctamente';
		END
END;
GO
-- Test de Eliminar Cliente
EXEC SP_EliminarCliente @IdCliente = 1;
GO
-- Obtener Cliente por ID
CREATE PROCEDURE SP_ObtenerClientePorId
	@IdCliente SMALLINT
AS
BEGIN
    SELECT TOP 1 IdCliente, Nombre, Apellido, DUI, Telefono, CorreoElectronico, Direccion
    FROM Cliente
	WHERE IdCliente = @IdCliente;
END;
GO
-- TestO btener Cliente por ID
EXEC SP_ObtenerClientePorId @IdCliente = 1
GO
```
