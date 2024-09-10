# AUTENTICACION - BACK-END

## PARTE 1 - LOGICA DE LA AUTENTICACION DE USUARIO
**¿Qué es Autenticación?**

Es el mecanismo que permite afirmar que la persona que esta ingresando al sistema es quien dice ser.

**¿Cómo Funciona?**

Se aceptan las credenciales ingresadas por el usuario ***(usuario – contraseña)*** y se validan contra una base de datos, el sistema operativo, un servicio web, u otro mecanismo definido según el tipo de autenticación.

![image](https://github.com/user-attachments/assets/64a3dfc7-7ff4-461e-9ed1-b8eb7a39603b)

## PARTE 2 - CONFIGURACION DE LA BASE DE DATOS
**Paso 1:** Codificar y ejecutar el Procedimiento almacenado para autenticar las credenciales (telefono y clave).
```sql
-- SELECCIONAR BASE DE DATOS
USE ElParaisalDB;
GO
-- CREAR VALIDACION DE CREDENCIALES
CREATE PROCEDURE SP_AutenticarCredenciales
	@Telefono CHAR(8),
	@Clave VARCHAR(250)
AS
BEGIN
	SELECT TOP 1 IdEmpleado, IdCargo, Nombre, Apellido, Telefono
    FROM Empleado
    WHERE Telefono = @Telefono AND Clave = @Clave
END;
-- PROBAR AUTENTICACION
GO
EXEC SP_AutenticarCredenciales @Telefono='98001234', @Clave='26d6a8ad97c75ffc548f6873e5e93ce475479e3e1a1097381e54221fb53ec1d2';
GO
```
![image](https://github.com/user-attachments/assets/d5a6c8c2-4ab3-4eea-819c-122914d2f441)

**Resultado:** El SP solo debe devolver 1 registro del usuario.

![image](https://github.com/user-attachments/assets/ecad8c1a-b685-47ad-82f8-45cd07a211a2)

**NOTA:** es importante aclarar que las el juego de credenciales (usuario y contraseña) es distinto en cada sistema, incluso puede ser la credenciales solamente un pin unico o clave secreta unica. 


## PARTE 2 - CONFIGURACION DE LA CAPA DAL

En este paso se creara un nuevo metodo en la clase de usuarios(Empleados,Personal o Usuario) del software. 

![image](https://github.com/user-attachments/assets/166e1736-e6df-402b-abcb-916ed8539112)


**Paso 1:** Abrir el archivo de la clase de la capa DAL.

![image](https://github.com/user-attachments/assets/cf16d387-b06d-4132-b6aa-5f31b5fe7577)

**Paso 2:** Ubicarse abajo de los metodos de busqueda y crear un nuevo metodo publico de acceso a datos, llamado **AutenticarCredenciales()**.

```csharp
#region Autenticacion
public static Empleado AutenticarCredenciales(Empleado pEmpleado)
{
    Empleado obj = new Empleado();

    SqlCommand comando = ComunDB.ObtenerComando();
    comando.CommandType = CommandType.StoredProcedure;
    comando.CommandText = "SP_AutenticarCredenciales";
    comando.Parameters.AddWithValue("@Telefono", pEmpleado.Telefono);
    comando.Parameters.AddWithValue("@Clave", pEmpleado.Clave);

    SqlDataReader reader = ComunDB.EjecutarComandoReader(comando);
    while (reader.Read())
    {
        // Orden de las columnas depende de la Consulta SELECT utilizada
        obj.IdEmpleado = reader.GetInt16(0); // Columna [0] cero
        obj.IdCargo = reader.GetByte(1); // Columna [0] cero
        obj.Nombre = reader.GetString(2);  // Columna [1] uno
        obj.Apellido = reader.GetString(3); // Columna [2] dos
        obj.Telefono = reader.GetString(4); // Columna [4] cuatro
    }
    return obj;
}
#endregion
```

![image](https://github.com/user-attachments/assets/426619f1-c540-4165-adc4-7abf2683e3ef)

## PARTE 3 - CONFIGURACION DE LA CAPA BL

En este paso se creara un nuevo metodo en la clase de usuarios(Empleados,Personal o Usuario) del software. 

![image](https://github.com/user-attachments/assets/48d1f122-7a80-44b7-b22b-232e64d07fc8)

**Paso 1:** Abrir el archivo de la clase de la capa BL.

![image](https://github.com/user-attachments/assets/65bae78b-f9ab-4e98-8991-96b47735730a)


**Paso 2:** Ubicarse abajo de los metodos de Bucar y crear un nuevo metodo publico para conectar el acceso a datos, llamado **AutenticarCredenciales()**.

```csharp
public Empleado AutenticarCredenciales(Empleado pEmpleado)
{
    pEmpleado.Clave = CifrarHashSha256(pEmpleado.Clave);
    return EmpleadoDAL.AutenticarCredenciales(pEmpleado);
}
```

![image](https://github.com/user-attachments/assets/6b5ec932-43a2-419f-9b9d-ea262677f46b)

