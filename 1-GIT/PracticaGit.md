# PROYECTO COLABORATIVO
El siguiente proyecto se divide en 4 partes, para su desarrollo se debe asignar una o mas partes a cada miembro del equipo.

**Estructura:**
- PARTE 1: Creacion de proyecto en Git con SCRUM.
- PARTE 2: Creacion de arquitectura N-Capas del proyecto.
- PARTE 3: Asociacion del proyecto de Visual Studio con Git (Azure Repos).
- PARTE 4: Clonacion del proyecto desde Git.

## PARTE 1: Creacion de proyecto en Git con SCRUM.
**Paso 1:** Abrir en el navegador Azure DevOps.

**Paso 2:** Crear un proyecto de tipo SCRUM con control de versiones GIT.
**Nombre:** es importante, no incluir espacios en el nombre del proyecto.
```
SistemaElParaisal
```
**Descripcion:**
```
Proyecto de sistema de ventas para el comedor "El Paraisal".
```
![image](https://github.com/user-attachments/assets/43d98e32-42a0-4162-b33e-0a09753452a5)
**Resultado:** Luego dirigirse a la organizacion. 
![image](https://github.com/user-attachments/assets/5a4d03bb-3ed9-4409-9acb-66730af37bff)

**Paso 3:** Ir a la configuracion de la organizacion.
![image](https://github.com/user-attachments/assets/39a8a0b2-6919-46ff-a6c9-b88737198aea)
**Paso 4:** Acceder al menu **Users** y luego dar clic en Add Users.
![image](https://github.com/user-attachments/assets/f04b34bb-1309-4776-b1c7-1a62f96ab225)
**Paso 5:** Agregar a los miembros de equipo con permiso basic y agregarlos como administradores del proyecto. (Incluir al correo: moisesescobar50@outlook.com)
![image](https://github.com/user-attachments/assets/b9894143-91b0-4fa3-a775-48373ec206f0)
**Resultado:**
![image](https://github.com/user-attachments/assets/f6c10988-8918-4ccd-b028-b7bbe1baebc1)

## PARTE 2: Creacion de arquitectura N-Capas del proyecto.
**Paso 1:** Abrir Visual Studio y Crear un nuevo proyecto.
![image](https://github.com/user-attachments/assets/2bea8f03-9bc9-49ce-933e-f8e718fcff56)
**Paso 2:** Seleccionar Crear una solucion en blanco y dar clic en Siguiente.
![image](https://github.com/user-attachments/assets/118c92ee-78f1-4034-92e1-57a3d0681435)
**Paso 3:** Nombrar a la solucion en blanco y dar clic en Crear.
```
SistemaElParaisal
```
![image](https://github.com/user-attachments/assets/72dd07f4-2221-4bd3-8710-d478c4b8ba8e)
**Resultado:**
![image](https://github.com/user-attachments/assets/160602de-253a-4773-8372-a791a44492bb)
**Paso 4:** Dar clic derecho en la solucion y agregar un Nuevo proyecto. 
![image](https://github.com/user-attachments/assets/771e4004-c252-4187-ae31-dd4af2db70d1)
**Paso 5:** Seleccionar una **Biblioteca de clases (.NET Framework)** y dar clic en Siguiente.
![image](https://github.com/user-attachments/assets/87e1ae24-b91a-438a-be88-f5d240c95c21)
**Paso 6:** Nombrar a la **Biblioteca de clases (.NET Framework)** y dar clic en Crear.
```
SistemaElParaisal.EN
```
![image](https://github.com/user-attachments/assets/54f87cf6-99a6-4e38-b183-90734026eb6d)

**NOTA:** Eliminar el archivo Class1.cs siempre, solo aplicar para **Biblioteca de clases (.NET Framework)**
![image](https://github.com/user-attachments/assets/979b345e-670a-4b93-a154-2cec927a44c8)

**Paso 7:** Crear como **Biblioteca de clases (.NET Framework)** las capas:
- SistemaElParaisal.DAL
- SistemaElParaisal.BL

**Resultado:**
![image](https://github.com/user-attachments/assets/222880e5-db41-4416-b038-f99beeacd38c)
**Paso 8:** Dar clic derecho en la solucion y agregar un Nuevo proyecto. 
**Paso 9:** Seleccionar un **Proyecto de prueba unitaria(.NET Framework)** y dar clic en Siguiente.
![image](https://github.com/user-attachments/assets/632f3e8a-4a1a-4858-9a35-52b13f015e02)

**Paso 10:** Nombrar al **Proyecto de prueba unitaria(.NET Framework)** y dar clic en Crear.
```
SistemaElParaisal.PruebasUnitarias
```
![image](https://github.com/user-attachments/assets/2c2dcaca-d91e-4515-980c-f1f5b1d81468)
**NOTA:** Eliminar el archivo UnitTest1.cs siempre, solo aplicar para **Proyecto de prueba unitaria(.NET Framework)**
![image](https://github.com/user-attachments/assets/0a83c273-5a3c-4ad9-b2fa-250f386186bd)

**Resultado:**
![image](https://github.com/user-attachments/assets/8257b780-509a-4374-86b3-c82fc698620a)


## CONFIGURACION DE GIT EN VS: Aplica para todos los miembros del equipo. 
**Paso 1:** En Visual Studio acceder al menu de **Herramientas > Opciones**
![image](https://github.com/user-attachments/assets/f27af871-4db5-490f-ab4e-aacfc8645330)
**Paso 2:** Buscar en la ventana, **Control de codigo fuente > Configuracion Global de Git** y en esta seccion configurar el **Nombre de usuario y Correo electronico**, al finalizar dar clic en **Aceptar**.
![image](https://github.com/user-attachments/assets/01e2e2e7-f766-485c-ac2f-08dfb6945072)
**NOTA:** Esta configuracion es necesaria para identificar quien modifica archivos en cualquier proyecto modificado en la computadora.

## PARTE 3: Asociacion del proyecto de Visual Studio con Git (Azure Repos).
**Paso 1:** Acceder al proyecto **SistemaElParaisal** en Azure DevOps.
**Paso 2:** Acceder a la opcion **Repos > Files** del proyecto **SistemaElParaisal** en Azure DevOps.
![image](https://github.com/user-attachments/assets/3f08fbdf-e123-4e7e-875e-483bdba892ed)
**Resultado:**
![image](https://github.com/user-attachments/assets/6bd7a241-237a-4b44-ac62-572cb5ff6ba5)
**Paso 3:** Hasta el final de la pagina web, Configurar el **.gitignore** como **"Visual Studio"** y dar clic en **Initialize**..
![image](https://github.com/user-attachments/assets/a7a9ed5f-615e-4372-b049-6e6d371b2eed)
**Paso 5:** Dar clic en la opcion Clone.
![image](https://github.com/user-attachments/assets/204b2e7c-c348-4679-b462-0207f6b2b297)
**Paso 6:** Copiar la direccion HTTPS del repositorio.
![image](https://github.com/user-attachments/assets/e39f5fdd-9af9-4040-a8a8-5e83dd7becce)
### IMPORTANTE: REVISAR TENER ACCESO A INTERNET y GUARDA CAMBIOS EN VISUAL STUDIO.
**Paso 7:** En Visual Studio dar clic derecho en la solucion y seleccionar **Crear respositorio de Git**.
![image](https://github.com/user-attachments/assets/f1e952da-ce1e-44ab-9f74-82ce7084d806)
![image](https://github.com/user-attachments/assets/ef72e194-5243-49c1-b380-32fd2ab230eb)
**Paso 8:** Seleccionar la opcion, **Repositorio remoto existente**
![image](https://github.com/user-attachments/assets/71a670c2-839a-4872-a037-e02e6ef38044)
**Paso 9:** Dar clic en Crear y Enviar los Cambios
![image](https://github.com/user-attachments/assets/006794cc-8465-45fd-8029-b031837e3d7c)
**Paso 10:** Inicia sesion en Git Credential Manager con tu cuenta de Azure DevOps.
![image](https://github.com/user-attachments/assets/a82b40d5-41e1-4ce3-b514-a7704fe9fd42)
**Termina el inicio de sesion**
![image](https://github.com/user-attachments/assets/c641bc4f-4b71-46a3-87aa-0d9090344927)
**Paso 11:** Regresa al menu Repos de tu proyecto, ahora tendras 2 ramas, **main y master**. Por defecto Visual Studio sube los cambios en la rama master. 
![image](https://github.com/user-attachments/assets/13670b58-f3cf-4c4a-9be6-696e3f28ab2c)
**Paso 12:** Accede a la rama **master** y tendras los archivos del proyecto.
![image](https://github.com/user-attachments/assets/a24d98d8-c279-42a4-880e-a9f0e196262f)

## PARTE 4: Clonacion del proyecto desde Git.
**IMPORTANTE:** Este paso no aplica para el miembro del equipo que creo el proyecto y lo subio al repositorio de Azure DevOps.

**Paso 1:** Abrir en el navegador el proyecto de Azure DevOps, dirigirse a **Repos > Files** en la rama **master**.
![image](https://github.com/user-attachments/assets/58e37d80-921f-4645-b6a5-74bbd18eb9dc)
**Paso 2:** Dar clic en la opcion Clone.
![image](https://github.com/user-attachments/assets/16f4ea39-4383-42c2-9043-6875c1126864)
**Paso 3:** Copiar la direccion HTTPS del repositorio.
![image](https://github.com/user-attachments/assets/4f24f3ff-e0c4-4ccf-9474-91322cf80e46)
**Paso 4:** Abrir Visual Studio y seleccionar Clonar un repositorio.
![image](https://github.com/user-attachments/assets/32abb7af-369b-4116-bc44-d65d16e0f09a)
**Resultado:**
![image](https://github.com/user-attachments/assets/c8d320b3-d719-4507-8d3c-6a5f7739b344)
**Paso 5:** Pegar la direccion HTTPS del repositorio y dar clic en Clonar.
![image](https://github.com/user-attachments/assets/508f9055-f72e-42b5-a377-74bee6fe4f23)
**Resultado:** No apareceran los archivos del proyecto, porque si nos fijamos en la esquina inferior derecha, estamos en la rama **main**
![image](https://github.com/user-attachments/assets/e7789ab7-930d-4216-8980-d88ebcaee57b)
**Paso 6:** Dar clic en la rama **main**
![image](https://github.com/user-attachments/assets/0258dada-f392-412d-bbe2-f1ad2c0affd3)
**Paso 7:** Dar clic en Remotos y seleccionar la rama **master**
![image](https://github.com/user-attachments/assets/882dad27-ca61-4e86-9de1-cdf5349efa3f)
**Resultado:** Ya aparecen los archivos de nuestro proyecto.
![image](https://github.com/user-attachments/assets/8816a678-36e2-4d77-b358-c67a75cad37e)
**Paso 8:** Dar doble clic sobre la **solucion** para abrir el proyecto. 
![image](https://github.com/user-attachments/assets/ab9ea0e9-88de-4072-a94c-9e30e7401e40)
**Resultado:**
![image](https://github.com/user-attachments/assets/3c755ad9-da18-4e98-b585-2c229648e7f3)
