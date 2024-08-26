# Guía Básica de Git

![th](https://github.com/user-attachments/assets/b485f4c9-e65e-4a6b-b7ea-3e22c692a869)

## Introducción
Git es un sistema de control de versiones distribuido que permite a los desarrolladores rastrear cambios en el código fuente durante el desarrollo de software. Es ampliamente utilizado para coordinar el trabajo entre varios programadores y gestionar proyectos de software.

## Conceptos Clave

### Repositorio
Un repositorio es un directorio que contiene todos los archivos y el historial de cambios del proyecto.

### Área de Preparación (Staging Area)
Es un área donde se colocan los cambios que serán incluidos en el próximo commit.

### Commit
Un commit es una instantánea del estado del proyecto en un momento dado.

### Ramas (Branches)
Las ramas permiten trabajar en diferentes versiones del proyecto simultáneamente.

### Árbol de trabajo y staging (o índice)

En Git, el directorio en el que está trabajando, que está bajo el control de Git, se llama árbol de trabajo.

Con el comando `git add` enviamos los cambios a staging (o índice), que es un estado intermedio en el que se van almacenando los archivos a enviar en el commit.<br>
Finalmente con `git commit` lo enviamos al repositorio local.

Si queremos colaborar con otros, con `git push` subimos los archivos a un repositorio remoto y mediante `git pull` podríamos extraer los cambios realizados por otros en remoto hacia nuestro directorio de trabajo.

Si comenzamos trabajando en remoto, lo primero que hacemos es un clon de la información en el directorio local.

![image](https://github.com/itcha-organization/git-tutorial/assets/83223664/48f8b23b-2eb9-4652-bf49-0847efe6fb0c)
