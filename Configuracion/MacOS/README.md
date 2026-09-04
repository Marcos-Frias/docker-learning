# Configuracion de Entorno para macOS

Esta guía explica cómo instalar y optimizar **Docker Desktop** en macOS, cubriendo las diferencias entre chips **Apple Silicon (M1/M2/M3/M4)** e **Intel**, junto a las configuraciones clave de rendimiento y emulación para desplegar luego en servidores Linux de producción.

---

## 🧠 Concepto Clave: ¿Cómo funciona Docker en macOS?

Los contenedores de Docker dependen de características exclusivas del kernel de Linux (*cgroups* y *namespaces*). Como macOS utiliza el kernel Darwin:

* Docker Desktop levanta en segundo plano una **máquina virtual Linux optimizada**.
* Los comandos que ejecutas en tu terminal de macOS (`docker run`, `docker compose`) se comunican con esa máquina virtual transparente e inmediatamente.
* Por este motivo, en macOS instalamos **Docker Desktop**, que ya incluye el demonio, el cliente CLI, Docker Compose y Docker Buildx.

---

## 📋 Requisitos Previos

1. **Versión del sistema:** macOS 12 (Monterey) o superior (recomendado macOS 14 Sonoma o macOS 15+).
2. **Memoria RAM:** Mínimo 4 GB (se recomiendan 8 GB o más, ya que la máquina virtual reservará parte de ella).
3. **Identificar tu arquitectura de procesador:**
   * Haz clic en el ícono de **Apple ()** en la esquina superior izquierda > **Acerca de esta Mac**.
   * Identifica si tienes un chip **Apple (M1, M2, M3, M4...)** o procesador **Intel**.

> [!NOTE]
> **Licencia de Docker Desktop:** Es 100% gratuita para uso personal, estudiantes, proyectos de código abierto y pequeñas empresas (menos de 250 empleados y menos de 10 millones de dólares en ingresos anuales).

---

## Método 1: Instalación gráfica (Recomendado)

1. Descarga el instalador oficial según la arquitectura de tu Mac:
   * [Descargar para Mac con chip Apple Silicon (M1/M2/M3/M4)](https://desktop.docker.com/mac/main/arm64/Docker.dmg)
   * [Descargar para Mac con procesador Intel](https://desktop.docker.com/mac/main/amd64/Docker.dmg)

2. Abre el archivo descargado (`Docker.dmg`) y arrastra el ícono de **Docker** a la carpeta **Aplicaciones**.

3. Abre **Docker** desde tu Launchpad o Spotlight (`Cmd + Espacio` y escribe `Docker`).

4. macOS te solicitará permisos administrativos para instalar componentes de red y sockets; ingresa tu contraseña de usuario o usa Touch ID.

---

## Método 2: Instalación vía Terminal (Usando Homebrew)

Si prefieres usar el gestor de paquetes [Homebrew](https://brew.sh/), ejecuta:

```bash
brew install --cask docker
```

Una vez finalizada la descarga, inicia la aplicación:

```bash
open -a Docker
```

---

## Paso Crucial: Ajustes de Rendimiento para Desarrollo (Settings)

Para evitar que tu Mac se vuelva lenta y acelerar la lectura de volúmenes (carpetas compartidas con contenedores), abre Docker Desktop, haz clic en el engrane de **Settings (Configuración)** en la esquina superior derecha y ajusta lo siguiente:

### 1. Motor de Virtualización y Archivos (General & Resources)

* En **General**:
  * Marca: **Use Virtualization framework** (usa el hipervisor nativo de Apple en lugar de QEMU antiguo).
* En **Resources > File sharing**:
  * Selecciona: **VirtioFS**. Es el sistema de sincronización de archivos más rápido para macOS; evita que proyectos pesados (como Node.js con `node_modules` o PHP) se sientan lentos al montar volúmenes.

### 2. Soporte Rosetta para Apple Silicon (Solo chips M1/M2/M3/M4)

Si tienes un Mac con Apple Silicon, muchas imágenes de producción antiguas solo existen para arquitectura Intel (`amd64`).

* Ve a **Features in development** o **General** (según tu versión).
* Activa la casilla: **Use Rosetta for x86/amd64 emulation on Apple Silicon**.
* Si el sistema te lo pide, instala Rosetta 2 en tu terminal ejecutando:

  ```bash
  softwareupdate --install-rosetta --agree-to-license
  ```

---

## ⚠️ El "Gran Tropiezo" al pasar a Producción (Multi-Arquitectura)

> [!WARNING]
> Si estás en una Mac con Apple Silicon (M1-M4) y construyes una imagen con `docker build -t mi-app .`, esa imagen se compilará de forma nativa para **ARM64**.
>
> Si luego subes esa imagen a un servidor Linux en producción que usa procesadores tradicionales **Intel/AMD (x86_64 / amd64)**, el servidor **fallará con un error de formato de binario**.

### ¿Cómo solucionarlo? (La forma profesional)

Gracias a **Docker Buildx** (incluido por defecto), puedes indicar explícitamente la plataforma de destino para producción:

```bash
docker build --platform linux/amd64 -t mi-app:produccion .
```

O si necesitas compilar para ambas arquitecturas a la vez:

```bash
docker buildx build --platform linux/amd64,linux/arm64 -t mi-usuario/mi-app:latest --push .
```

---

## Verificación de la instalación

Abre tu terminal favorita (Terminal de macOS, iTerm2 o la integrada de VS Code) y valida que las utilidades respondan:

```bash
docker --version
docker compose version
docker buildx version
```

Prueba la ejecución de tu primer contenedor:

```bash
docker run --rm hello-world
```

Si todo está configurado correctamente, verás el mensaje de bienvenida de Docker indicando que el demonio está activo.

---

## 🛠️ Problemas Frecuentes en macOS

### 1. Conflicto con el Puerto 5000 (AirPlay Receiver)

* **Síntoma:** Al levantar un contenedor que use el puerto `5000` (muy común en APIs de Flask, Python o Docker Registry), da error de puerto en uso.
* **Causa:** Desde macOS Monterey, la función **Receptor AirPlay** de Apple escucha por defecto en el puerto `5000`.
* **Solución:** Ve a **Ajustes del Sistema** > **General** > **AirDrop y Handoff** y desactiva **Receptor AirPlay** (o cambia el puerto en tu archivo `docker-compose.yml` a `5001:5000`).

### 2. El comando `docker` no se reconoce tras instalar

* Si instalaste arrastrando el archivo `.dmg`, asegúrate de haber abierto la aplicación **Docker** al menos una vez para que cree los enlaces simbólicos en `/usr/local/bin` o `/opt/homebrew/bin`.

### 3. Consumo excesivo de batería o RAM

* Ve a **Settings > Resources** en Docker Desktop y limita manualmente la cantidad de núcleos de CPU y memoria RAM que puede consumir la máquina virtual (por ejemplo, asignarle 4 GB de RAM si tu Mac tiene 16 GB).

---

## 💡 Alternativas Ligeras a Docker Desktop en macOS

Si en algún momento trabajas en una empresa donde las políticas de licencia restrinjan Docker Desktop, o buscas algo más ligero que consuma menos batería, existen alternativas compatibles con los mismos comandos:

* **[OrbStack](https://orbstack.dev/):** Alternativa comercial sumamente rápida y con muy bajo consumo de recursos en macOS.
* **[Colima](https://github.com/abiosoft/colima):** Herramienta Open Source por línea de comandos que levanta un entorno Docker nativo usando QEMU/VZ sin interfaz gráfica (`brew install colima docker`).

---

## 🗑️ Cómo desinstalar Docker por completo en macOS

Si necesitas limpiar el sistema:

1. Abre **Docker Desktop**.
2. Haz clic en el ícono del **Escarabajo / Bug (Troubleshoot)** en la barra superior.
3. Selecciona **Uninstall Docker Desktop**.
4. Si prefieres hacerlo manual o usaste Homebrew:

   ```bash
   brew uninstall --cask docker
   rm -rf ~/Library/Containers/com.docker.docker
   rm -rf ~/Library/Application\ Support/Docker\ Desktop
   rm -rf ~/.docker
   ```

---
