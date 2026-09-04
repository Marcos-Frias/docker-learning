# Configuracion de Entorno para Linux (Ubuntu, Debian)

Esta guía explica paso a paso cómo instalar la versión oficial de **Docker Engine**, junto a **Docker Compose** y **Docker Buildx**, configurando el entorno con buenas prácticas listas para desarrollo y producción.

---

## 🧭 ¿Qué vamos a instalar y por qué?

Aunque solo **Docker Engine** es estrictamente obligatorio para arrancar contenedores, en un flujo de trabajo real instalamos la suite oficial completa:

1. **Docker Engine (`docker-ce`, `docker-ce-cli`, `containerd.io`):** El motor principal que ejecuta y administra contenedores.
2. **Docker Compose (`docker-compose-plugin`):** La herramienta estándar para orquestar múltiples contenedores (ej. base de datos + backend + frontend) usando archivos `docker-compose.yml`.
3. **Docker Buildx (`docker-buildx-plugin`):** El constructor moderno de imágenes con soporte para compilación multiplataforma (AMD64 y ARM64) y caché avanzada.

---

## 📋 Requisitos Previos

- Distribución Linux de 64 bits (`amd64`/`x86_64`) o ARM (`arm64`).
- Acceso a una cuenta con permisos administrativos (`sudo`).
- Conexión a Internet.
- Si usas una máquina virtual (VirtualBox, VMware, KVM), asegúrate de tener la **virtualización anidada** activada en tu hipervisor.

---

## Paso 1: Eliminar paquetes conflictivos

Ubuntu y Debian a menudo incluyen paquetes genéricos o desactualizados que entran en conflicto con los repositorios oficiales de Docker. Ejecuta este comando para limpiar cualquier versión previa:

```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do
  sudo apt-get remove -y $pkg
done
```

> [!NOTE]
> Es normal si el sistema responde que ninguno de estos paquetes estaba instalado.

---

## Paso 2: Preparar el gestor de paquetes

Actualiza los repositorios del sistema e instala las utilidades necesarias para gestionar repositorios seguros vía HTTPS:

```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
```

---

## Paso 3: Configurar el Repositorio Oficial de Docker

Selecciona la opción correspondiente a la base de tu sistema:

### Opción A: Para Ubuntu y derivados (Linux Mint, Pop!_OS, Zorin OS, Elementary)

1. Descarga la clave GPG de Docker para Ubuntu:

   ```bash
   sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
   sudo chmod a+r /etc/apt/keyrings/docker.asc
   ```

2. Registra el repositorio oficial (compatible con derivados mediante `UBUNTU_CODENAME`):

   ```bash
   echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
     $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
     sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

---

### Opción B: Para Debian y derivados (Raspberry Pi OS, LMDE, MX Linux, Kali)

1. Descarga la clave GPG de Docker para Debian:

   ```bash
   sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
   sudo chmod a+r /etc/apt/keyrings/docker.asc
   ```

2. Registra el repositorio oficial:

   ```bash
   echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
     $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
     sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

---

## Paso 4: Instalar Docker Engine y plugins

Sincroniza el catálogo de paquetes e instala el paquete completo:

```bash
sudo apt-get update
sudo apt-get install -y \
  docker-ce \
  docker-ce-cli \
  containerd.io \
  docker-buildx-plugin \
  docker-compose-plugin
```

Habilita los servicios para que arranquen automáticamente con el sistema:

```bash
sudo systemctl enable --now docker
sudo systemctl enable --now containerd
```

---

## Paso 5: Configuración para Producción (Rotación de Logs)

> [!IMPORTANT]
> **Crítico en servidores:** Por defecto, Docker no limita el crecimiento de los logs de los contenedores. Un contenedor con errores continuos puede generar decenas de gigabytes de texto hasta **llenar el disco duro y congelar el servidor**.

Establece una política global de rotación creando el archivo de configuración del demonio:

```bash
sudo tee /etc/docker/daemon.json > /dev/null <<EOF
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "20m",
    "max-file": "3"
  }
}
EOF
```

*Con esta regla, ningún contenedor acumulará más de 60 MB en logs (3 archivos de 20 MB como máximo).*

Aplica los cambios reiniciando el servicio:

```bash
sudo systemctl restart docker
```

---

## Paso 6: Configurar permisos de usuario (Ejecutar sin `sudo`)

> [!WARNING]
> **Consideración de seguridad:** Dar acceso al socket de Docker a un usuario equivale técnicamente a otorgarle permisos de `root` en la máquina (un contenedor con privilegios puede montar el disco raíz del host). En servidores compartidos con múltiples usuarios, mantén el uso de `sudo` o investiga [Rootless Docker](https://docs.docker.com/engine/security/rootless/).

Para entornos de desarrollo o tu propio servidor:

1. Añade tu usuario al grupo `docker`:

   ```bash
   sudo groupadd -f docker
   sudo usermod -aG docker "$USER"
   ```

2. Aplica el cambio a tu terminal activa sin necesidad de reiniciar:

   ```bash
   newgrp docker
   ```

---

## Paso 7: Comprobar la instalación

1. Ejecuta el contenedor de prueba oficial:

   ```bash
   docker run --rm hello-world // *(La bandera `--rm` elimina el contenedor automáticamente tras finalizar su ejecución).*
   ```

2. Verifica que las herramientas auxiliares estén operativas:

   ```bash
   docker compose version
   docker buildx version
   ```

---

## 🛠️ Resolución de Problemas Frecuentes

### 1. `permission denied while trying to connect to the Docker daemon socket`

- **Solución:** Tu usuario aún no tiene activos los permisos del grupo `docker`. Ejecuta `newgrp docker` o cierra sesión completamente (en SSH desconéctate y vuelve a entrar).

### 2. Error 404 al ejecutar `apt-get update` tras agregar el repositorio

- **Solución:** Común en distribuciones derivadas (Linux Mint, Pop!_OS) cuando el script no resuelve el nombre base de Ubuntu/Debian. Abre el archivo `/etc/apt/sources.list.d/docker.list` y reemplaza el nombre de tu distribución por la versión base correspondiente (ejemplo: `noble` para Ubuntu 24.04, `jammy` para 22.04 o `bookworm` para Debian 12).

### 3. Docker y el Firewall (UFW)

- **Solución:** En Ubuntu, Docker se comunica directamente con `iptables` y por defecto puede ignorar las reglas de bloqueo configuradas en `ufw`. Si publicas puertos en producción (ej. `-p 8080:80`), esos puertos quedarán abiertos a Internet a menos que enlaces explícitamente a localhost (`-p 127.0.0.1:8080:80`).

---

## 🗑️ Cómo desinstalar Docker por completo

Si alguna vez necesitas revertir el proceso y dejar el sistema limpio:

```bash
# 1. Desinstalar paquetes
sudo apt-get purge -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras

# 2. Eliminar volúmenes, imágenes y redes (¡Atención: borra todos tus contenedores y datos!)
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
sudo rm -rf /etc/docker/daemon.json
sudo rm -f /etc/apt/sources.list.d/docker.list
sudo rm -f /etc/apt/keyrings/docker.asc
```
