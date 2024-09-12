# ANOTACIONES DE CLASE .NET

- **NOTA 1:**  Las anotaciones se utilizaran para validar **"automaticamente"** el **modelo** (clase) al generar las View con una **Plantilla**.
- **NOTA 2:**  Las anotaciones solo seran necesarias para los modelos que se realizaran acciones de **Guardar** o **Editar**.
- **NOTA 3:**  Las anotaciones **no aplican** para **catalogos**. 

Las anotaciones de clase funcionan como reglas que se deben aplicar o validar al interactuar con el modelo de datos, este componente es utilizado en la mayoria de casos al trabajar con el ORM EntityFrameWork, el cual permite crear una base de datos mediante clases y acceder a las instrucciones SQL desde .NET.

**ORM:** Object Relational Model.

![getfile](https://github.com/user-attachments/assets/1f634eba-277c-4d8b-89c5-0d397cf17999)

# DOCUMENTACION Y EJEMPLOS DE ANOTACIONES

Existe una extensa lista de anotaciones en .NET, puedes consultarlas [aquí](https://www.bytehide.com/blog/data-annotations-in-csharp). 

### Lista de anotaciones:

![image](https://github.com/user-attachments/assets/75007444-ef9d-4b40-a445-8faa32011ae2)

### Descripcion y Ejemplos:

![image](https://github.com/user-attachments/assets/b018ca89-a102-4a6f-bac7-5a66dc1cc022)

## PARTE 1 - Configuracion del modelo 

**Paso 1:** Ir a la capa **SistemaElParaisal.EN** y abrir la clase **"Empleado.cs"** desde el **Explorador de Soluciones**.

![image](https://github.com/user-attachments/assets/5e4b6ef7-8bb6-4456-9416-9358fa807b58)

**Paso 2:** Agregar la primer anotacion a la propiedad **IdEmpleado**.

```csharp
[Display(Name = "Id")]
```

![image](https://github.com/user-attachments/assets/2fb2d4f7-9a5a-44cc-a359-2b10ff5fe1f4)

**Paso 3:** Marcara un error, dar clic derecho sobre el icono de foco en **[Display(Name = "Id")]** y dar clic sobre **Agregue referencia**.

![image](https://github.com/user-attachments/assets/603717af-8def-4274-84ae-9f2362cb884c)

**Resultado** Se agrego la referencia al archivo

![image](https://github.com/user-attachments/assets/5cba4a22-bc5c-41ee-86e9-6ca10217c46a)

**Paso 4:** Ordenar las referencias y agregar comentario, para tener organizado el codigo.  Este debe ser el **paso 2** siempre.

```csharp
// Referencias
using System.ComponentModel.DataAnnotations;
```

![image](https://github.com/user-attachments/assets/f05920f6-0fcd-473d-b996-6296288bcfed)

**Paso 5:** Agregar todas las anotaciones, **excepto a las propiedades virtuales o listas** (List<>).

![image](https://github.com/user-attachments/assets/87b5517c-bd89-4ea8-8a1a-fae08a50eb5e)

**Resultado:**

![image](https://github.com/user-attachments/assets/bc4e0b8b-c99c-4c96-bab4-d571d09a007f)

