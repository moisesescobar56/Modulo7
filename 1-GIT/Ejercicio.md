# EJERCICIO
 Realizar lo siguiente:
1. Agregar a la solucion un proyecto de tipo "Aplicacion de Windows Forms (.NET Framework)".
2. Configurar la arquitectura del proyecto como el diagrama adjunto (Segun dependencia de las flechas).
3. Sincronizar los cambios con el repositorio.

## 1. Agregar a la solucion un proyecto de tipo **"Aplicacion de Windows Forms (.NET Framework)"**.
```
SistemaElParaisal.UI.WinForms
```
![image](https://github.com/user-attachments/assets/c2214f6a-4043-4fb7-b9e3-843172ae4fe4)
## 2. Configurar la arquitectura del proyecto como el diagrama adjunto (Segun dependencia de las flechas).
![2023 G2_G5 - Desarrollo de CRUD en aplicación de Windows Forms](https://github.com/user-attachments/assets/48f5fc05-e88b-4fcf-84b5-38fffc9ad4d3)
## 3. Sincronizar los cambios con el repositorio.

**Paso 1:** Guardar todos los cambios de Visual Studio.

**Paso 2:** Compilar la **"Solucion de visual studio"** y comprbar que hay cero proyectos con errores.
![image](https://github.com/user-attachments/assets/9b1023a3-f9a1-4a67-8a92-40219f0247e4)

**IMPORTANTE:** Si el proyecto tiene errores, se deben corregir 

**Paso 3:** En Visual Studio ir al menu **Ver > Cambios de GIT**
![image](https://github.com/user-attachments/assets/fc5ecfc4-8123-419b-871e-a5b3937dc7f4)

**Paso 4:** Dar clic en el boton **"+"** para confirmar los archivos que se subiran. 
![image](https://github.com/user-attachments/assets/02b4d430-c621-45e2-b220-f65893a2ef46)

**Paso 5:** Agregar un mensaje que describa las acciones realizadas en el dia. 
```
Se agrego la capa UI de escritorio a la solucion. Se configuraron las dependencias de la arquitectura de N-Capas del proyecto. 
```
![image](https://github.com/user-attachments/assets/8142629a-de1e-4883-b0ee-c9ff252e1282)
**Paso 6:** En el boton abajo del mensaje, seleccionar la opcion **"Confirmar todo y enviar/insertar"**. Y los cambios se enviaran al servidor.
![image](https://github.com/user-attachments/assets/abb1cb11-406b-4c60-b3d3-7dba85aa4b8c)
**Resultado:** Si vamos a Repos en la rama **master** desde Azure DevOps, comprobaremos que ya estan agregados los cambios y la capa UI.
![image](https://github.com/user-attachments/assets/b295b3af-f0f7-4428-a3ed-c799299ea50c)

**Paso 7:** Los otros miembros del equipo, desde Visual Studio deben ir a **Ver > Cambios de GIT**
![image](https://github.com/user-attachments/assets/b17f99c6-c9f9-48c2-a323-71f2fddf1351)

**Paso 8:** En la ventana de **"Cambios de GIT"**, primero verificar que estamos en la rama **master** y luego dar clic en el icono de descargar **"⬇"**, llamado **Extraer**.
![image](https://github.com/user-attachments/assets/febd39e3-77e7-4851-bbe0-42f120036f49)

**Resultado:** Podran confirmar que los cambios se han descargado. 
![image](https://github.com/user-attachments/assets/cc12e98e-2519-459e-b211-074a9b3ec806)
![image](https://github.com/user-attachments/assets/53f0918c-2dd9-4019-9a88-503751ce1ceb)


