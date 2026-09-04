# Configuracion de Entorno para Windows con WSL 2

Esta guía detalla cómo instalar y configurar **Docker Desktop** en Windows 10 y Windows 11 utilizando el motor moderno **WSL 2 (Windows Subsystem for Linux 2)**, junto a las configuraciones indispensables para evitar problemas de rendimiento y consumo desmedido de memoria RAM.

---

## 🧠 Concepto Clave: ¿Por qué es obligatorio usar WSL 2?

Los contenedores de Docker necesitan el kernel de Linux para funcionar. Históricamente, Windows usaba máquinas virtuales pesadas en Hyper-V. Hoy en día, la solución estándar y recomendada es **WSL 2**:

* **¿Qué es WSL 2?** Es una capa que ejecuta un **kernel de Linux real y optimizado** directamente dentro de Windows, sin el peso ni la lentitud de una máquina virtual tradicional.
* **Integración total:** Puedes ejecutar comandos de Docker tanto desde tu consola de Windows (**PowerShell** o **CMD**) como directamente desde tu terminal de Linux (**Ubuntu en WSL**).
* **Rendimiento:** WSL 2 ofrece acceso casi nativo al hardware, arranque instantáneo y soporte de virtualización avanzada.

---

## 📋 Requisitos Previos

1. **Sistema Operativo:**
   * **Windows 11** (64 bits: Home, Pro, Enterprise o Education).
   * **Windows 10** (64 bits: Home, Pro o Enterprise) versión 21H2 o superior (Compilación 19044+).
2. **Virtualización por hardware habilitada en el BIOS/UEFI:**
   * Abre el *Administrador de tareas* (`Ctrl + Shift + Esc`) > Pestaña **Rendimiento** > **CPU**.
   * Verifica que en la esquina inferior derecha diga: **Virtualización: Habilitada**. Si está deshabilitada, debes activarla en el BIOS de tu placa base (Intel VT-x o AMD-V/SVM).

---

## Paso 1: Instalar y Actualizar WSL 2

Para instalar WSL2 te recomendamos ver esta guia crea en Notion , para no repetir los pasos, ya que es un proceso largo y tedioso.

* [Guía de Instalación de WSL2 en Notion](https://app.notion.com/p/Gu-a-para-Instalar-WSL2-3c7045c9332c8086a780fcce196a6482?source=copy_link)

---

## Paso 2: Instalar Docker Desktop

### Opción A: Vía Terminal con WinGet (Recomendado)

Abre PowerShell y ejecuta:

```powershell
winget install Docker.DockerDesktop
```

### Opción B: Descarga Manual

1. Descarga el instalador oficial: [Docker Desktop para Windows](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe).
2. Ejecuta el archivo descargado (`Docker Desktop Installer.exe`).
3. En la pantalla de opciones, asegúrate de que la casilla **Use WSL 2 instead of Hyper-V (recommended)** esté **marcada**.
4. Sigue las instrucciones y, al finalizar, haz clic en **Close and restart** para reiniciar tu sesión de Windows.

---

## Paso 3: Configurar la Integración con tu distro de Linux

Una vez instalado, abre **Docker Desktop** desde el Menú Inicio:

1. Ve al icono del engrane (**Settings**) en la parte superior derecha.
2. En la pestaña **General**, verifica que esté marcada la opción:  
   `[x] Use the WSL 2 based engine`.
3. En la pestaña **Resources > WSL integration**:
   * Asegúrate de marcar `Enable integration with my default WSL distro`.
   * Si tienes instaladas distribuciones adicionales (por ejemplo, `Ubuntu`), activa el interruptor que aparece junto a su nombre.
4. Haz clic en **Apply & restart**.

---

## Paso 4: Ajustes Críticos de Rendimiento (Mentalidad de Producción)

Aquí es donde fallan la mayoría de los desarrolladores en Windows. Si no configuras estos dos aspectos, tu equipo se sentirá lento.

### 1. Limitar el consumo de RAM de WSL 2 (`.wslconfig`)

Por defecto, WSL 2 puede llegar a consumir hasta el 50% (o más) de toda la memoria RAM de tu computadora mediante el proceso `vmmem`, congelando Windows si levantas contenedores pesados.
Para ponerle un límite estricto:

1. Presiona `Win + R`, escribe `%USERPROFILE%` y presiona Enter (se abrirá tu carpeta de usuario en Windows).
2. Crea un archivo de texto llamado exactamente `.wslconfig` (ojo con el punto al inicio).
3. Pega el siguiente contenido (adapta la memoria según tu máquina; por ejemplo, si tienes 16 GB, asígnale 6 GB o 8 GB a WSL):

    ```ini
    [wsl2]
    memory=6GB
    processors=4
    swap=2GB
    ```

4. Guarda el archivo y reinicia el motor de WSL en PowerShell ejecutando:

   ```powershell
   wsl --shutdown
   ```
  
*(Docker se reiniciará automáticamente respetando este límite).*

### 2. La Regla de Oro del Sistema de Archivos (I/O Performance)

> [!CRITICAL]
> **Nunca coloques tus proyectos en carpetas de Windows como `C:\Users\tu-nombre\proyecto` si los vas a montar en contenedores.**
> La comunicación entre el sistema de archivos de Windows (NTFS) y el kernel de Linux de WSL 2 es extremadamente lenta para operaciones pesadas (como `npm install`, proyectos PHP o bases de datos).
>
> **La forma correcta:**
> Abre tu terminal de Ubuntu en WSL y clona tus proyectos dentro de la ruta nativa de Linux (por ejemplo en `~/proyectos` o `/home/tu-usuario/proyecto`). Puedes abrir VS Code directamente dentro de esa carpeta ejecutando `code .` desde la terminal de Ubuntu.

---

## Paso 5: Comprobar la instalación

Puedes verificar que Docker esté activo abriendo **PowerShell** o abriendo tu terminal de **Ubuntu (WSL)**. El comando responderá en ambos lugares:

1. Valida las herramientas instaladas:

   ```bash
   docker --version
   docker compose version
   docker buildx version
   ```

2. Ejecuta el contenedor de prueba:

   ```bash
   docker run --rm hello-world
   ```

Si ves el mensaje `Hello from Docker!`, tu entorno con WSL 2 está listo para trabajar.

---

## 🛠️ Resolución de Problemas Frecuentes en Windows

### 1. Error: `Hardware assisted virtualization and data execution protection must be enabled in the BIOS`

* **Solución:** La virtualización está desactivada en tu procesador. Reinicia la computadora, entra a la configuración del BIOS/UEFI y activa **Intel Virtualization Technology (VT-x)** o **AMD SVM / AMD-V**.

### 2. Error: `WSL 2 installation is incomplete` o `kernel update needed`

* **Solución:** Descarga manualmente el paquete de actualización del kernel de WSL 2 oficial de Microsoft ejecutando en PowerShell:

  ```powershell
  wsl --update --web-download
  ```

### 3. El proceso `vmmem` consume toda la CPU o RAM

* **Solución:** Cierra Docker Desktop y ejecuta `wsl --shutdown` en PowerShell. Revisa que hayas creado correctamente el archivo `.wslconfig` explicado en el Paso 4.

---

## 🗑️ Cómo desinstalar Docker por completo en Windows

Si necesitas limpiar el entorno:

1. Ve a **Configuración de Windows > Aplicaciones > Aplicaciones instaladas**.
2. Busca **Docker Desktop** y selecciona **Desinstalar**.
3. Si deseas eliminar todos los datos residuales de los contenedores en WSL:
   Abre PowerShell como administrador y desregistra las instancias de datos:

   ```powershell
   wsl --unregister docker-desktop
   wsl --unregister docker-desktop-data
   ```

---
