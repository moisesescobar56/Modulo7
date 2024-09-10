# CONFIGURACIÓN CAPA UI EN WINDOWS FORMS
## PARTE 1 - Creacion y configuracion de formularios, carpetas y clases
**Paso 1:** Abrir Visual Studio y Configurar los siguientes archivos:
- Administracion
  - AdminEmpleadoForm.cs
- Registro
  - RegistroEmpleadoForm.cs 
- Reportes
- ViewModels
- ToolsForms.cs

## Ver tutorial: https://youtu.be/8F7qcFjpcFY

**Resultado:**
![image](https://github.com/user-attachments/assets/7b916c41-6c11-4350-b11f-ca17ca008d96)

**NOTA:** en el video se crean los formularios ***"AdminClienteForm.cs"*** y ***"RegistroClienteForm.cs"***, pero en la practica se eliminaron, debido a que no se desarrollaran en esta guia. 

## PARTE 2 - Configuracion de la clase ***"ToolsForms.cs"***
**Paso 1:** abrir la clase **"ToolsForms.cs"**.

![image](https://github.com/user-attachments/assets/c668ebe5-8fa1-4392-a69e-528907784023)

**Paso 2:** agregar los using de las referencias de las bibliotecas a utilizar.
```csharp
///Referencias
using System.Windows.Forms;
```

![image](https://github.com/user-attachments/assets/0240ecf2-abb8-4136-a998-1341b2b2c3b9)

**Paso 3:** programar los metodos de las herramientas a implementar en la clase **"ToolsForms.cs"**.
```csharp
public static long ObtenerIdGrid(DataGridView pDataGridView)
{
    // Metodo para obtener el id de registro seleccionado en un DataGridView
    try
    {
        long idTabla = 0;
        // Obtener la PK del registro seleccionado
        idTabla = Convert.ToInt64(pDataGridView.Rows[pDataGridView.CurrentRow.Index].Cells[0].Value.ToString());
        return idTabla;
    }
    catch (Exception ex)
    {
        return 0;
    }
}

public static DialogResult MessageBoxConfirmar()
{
    return MessageBox.Show("¿Desea eliminar el registro seleccionado?", "Eliminar", MessageBoxButtons.YesNo, MessageBoxIcon.Question);
}
```
![image](https://github.com/user-attachments/assets/4fb100c5-4c0f-470f-9920-1e251fc77284)

**Paso 4:** minimizar o contraer la clase **"ToolsForms.cs"**.

![image](https://github.com/user-attachments/assets/b47ffbe7-15a9-45aa-ac47-e73e5f936e1e)

**Resultado:** ubircarse al final del codigo del archivo.
![image](https://github.com/user-attachments/assets/966091be-edc5-42a2-a984-169461b6c79d)

**Paso 5:** codificar una nuevo espacio de nombres, llamado **"ToolsForms"** y agregar un metodo extensivo de validacion para controles **"TextBox"** y **"ComboBox"** a la clase ***"ErrorProvider"*** nativa de .NET Framework

```csharp
namespace ToolsForms
{
    public static class MyExtensions
    {
        public static bool ValidarControl(this ErrorProvider errorProvider, Control pControl, string pMensaje, string pRegex = null)
        {
            if (pControl is TextBox)
            {
                if (pControl.Text == null || pControl.Text.Trim() == string.Empty)
                {
                    errorProvider.SetError(pControl, pMensaje);
                    return false; // dato invalido
                }
                if (pRegex != null && pRegex.Trim() != "")
                {
                    if (!System.Text.RegularExpressions.Regex.IsMatch(pControl.Text, pRegex))
                    {
                        errorProvider.SetError(pControl, "Formato invalido");
                        return false; // formato invalido
                    }
                }
            }
            if (pControl is ComboBox && ((ComboBox)pControl).SelectedIndex <= 0)
            {
                errorProvider.SetError(pControl, pMensaje);
                return false; // dato invalido
            }
            return true; // dato valido
        }
    }
}
```

**Resultado:**

![image](https://github.com/user-attachments/assets/f174cc32-eb26-4687-835a-19c9a7784306)

**NOTA:** una vez configurada esta seccion de la guia, se puede avanzar en el diseño de los formularios.

## EJEMPLOS

### **AdminEmpleadoFrom.cs**
![image](https://github.com/user-attachments/assets/1184cf67-7df5-4856-90e7-059b4330d9c2)

### **RegistroEmpleadoFrom.cs**
![image](https://github.com/user-attachments/assets/5d227dc7-b75c-4174-89e9-aad63a943571)


