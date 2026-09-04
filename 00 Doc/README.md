# Lección 0: Documentación y conceptos fundamentales de Docker

## 1. ¿Qué es Docker?

Docker es una plataforma de código abierto que permite **empaquetar, distribuir y ejecutar aplicaciones** en unidades ligeras llamadas **contenedores**. Un contenedor incluye todo lo necesario para que una aplicación se ejecute: código, runtime, bibliotecas, variables de entorno y configuraciones.

Docker apareció en 2013 y revolucionó la forma en que desarrollamos, desplegamos y escalamos aplicaciones al popularizar el uso de contenedores, una tecnología que existía en el kernel de Linux desde hacía años pero era compleja de usar.

### 1.1 Beneficios clave

- **Portabilidad**: la misma imagen se puede ejecutar en cualquier entorno con Docker (desarrollo, pruebas, producción).
- **Ligereza**: los contenedores comparten el kernel del host, consumen menos recursos que las máquinas virtuales y arrancan en segundos.
- **Estandarización**: elimina el clásico "en mi máquina funciona".
- **Aislamiento**: cada contenedor tiene su propio sistema de archivos, procesos y red, aunque comparte el kernel.
- **Escalabilidad**: facilita la orquestación con herramientas como Docker Swarm o Kubernetes.

## 2. Contenedores vs Máquinas Virtuales

| Característica | Contenedores | Máquinas Virtuales |
|----------------|--------------|---------------------|
| Aislamiento    | A nivel de proceso (namespaces) | A nivel de hardware (hipervisor) |
| Sistema operativo | Comparten kernel del host | Cada VM tiene su propio SO completo |
| Tamaño típico  | Megabytes | Gigabytes |
| Arranque       | Milisegundos a segundos | Minutos |
| Rendimiento    | Casi nativo, bajo overhead | Mayor overhead por virtualización |
| Portabilidad   | Muy alta (imagen portable) | Menor (depende del hipervisor) |

**Nota:** En Windows y macOS, Docker utiliza una máquina virtual ligera (con Linux) para ejecutar los contenedores, pero desde la perspectiva del usuario se comporta igual que en Linux nativo.

## 3. Arquitectura de Docker

Docker emplea una arquitectura cliente-servidor:

- **Docker Daemon (dockerd)**: proceso en segundo plano que gestiona imágenes, contenedores, redes y volúmenes. Escucha peticiones a través de una API REST (normalmente sobre socket Unix `/var/run/docker.sock` o TCP).
- **Docker Client (docker)**: interfaz de línea de comandos que se comunica con el daemon. Cada comando `docker` se traduce en llamadas a la API.
- **Registros**: almacenes de imágenes (como Docker Hub o registros privados). El daemon descarga imágenes de estos registros.

### 3.1 Componentes principales

- **Imagen**: plantilla de solo lectura con instrucciones para crear un contenedor. Se construye en capas (layers) a partir de un Dockerfile.
- **Contenedor**: instancia ejecutable de una imagen. Añade una capa de escritura sobre la imagen base.
- **Dockerfile**: archivo de texto con instrucciones para construir una imagen automáticamente.
- **Volúmenes**: mecanismo para persistir datos generados por contenedores y compartirlos entre ellos o con el host.
- **Redes**: permiten la comunicación entre contenedores y con el exterior (bridge, host, overlay, etc.).
- **Docker Compose**: herramienta para definir y ejecutar aplicaciones multi-contenedor mediante un archivo YAML.

## 4. ¿Cómo funciona Docker internamente?

Docker se apoya en tecnologías del kernel de Linux:

### 4.1 Namespaces (Espacios de nombres)
Aíslan la vista del sistema para cada contenedor:
- **PID**: procesos independientes (el contenedor ve su propio PID 1).
- **NET**: interfaces de red, direcciones IP y tablas de enrutamiento propias.
- **MNT**: sistema de archivos montado separado.
- **UTS**: nombre de host y dominio.
- **IPC**: comunicación entre procesos (colas de mensajes, semáforos).
- **USER**: usuarios y grupos.

### 4.2 Cgroups (Control Groups)
Limitan y monitorean los recursos que puede usar un contenedor (CPU, memoria, disco I/O, red). Sin cgroups, un contenedor podría acaparar todos los recursos del host.

### 4.3 Union Filesystems (OverlayFS, AUFS, etc.)
Permiten superponer capas de solo lectura (de la imagen) con una capa de escritura (del contenedor). Así se optimiza el almacenamiento y se comparten capas comunes entre contenedores.

## 5. Ciclo de vida de un contenedor

1. **Crear**: `docker create` o `docker run` (crea y arranca).
2. **Ejecutar**: `docker start` / `docker run`.
3. **Pausar/Reanudar**: `docker pause` / `docker unpause`.
4. **Detener**: `docker stop` (envía SIGTERM y, si no termina en un tiempo, SIGKILL).
5. **Eliminar**: `docker rm`.

El contenedor pasa por estados: `created`, `running`, `paused`, `exited`, `dead`.

## 6. Imágenes y capas

Las imágenes se construyen en capas. Cada instrucción en un Dockerfile crea una nueva capa. Esto permite:

- Reutilizar capas entre imágenes (por ejemplo, la capa de Ubuntu se comparte entre muchas imágenes).
- Descargas incrementales (solo se descargan las capas nuevas).
- Copia en escritura (copy-on-write): los contenedores comparten las capas base y solo guardan los cambios en su capa de escritura.

Ejemplo de Dockerfile básico:

```dockerfile
# Capa base
FROM ubuntu:22.04

# Capa 1: instalar nginx
RUN apt-get update && apt-get install -y nginx

# Capa 2: copiar archivos
COPY index.html /var/www/html/

# Capa 3: exponer puerto
EXPOSE 80

# Capa 4: comando de arranque
CMD ["nginx", "-g", "daemon off;"]
```

## 7. Registros de imágenes
Docker Hub: registro público por defecto. Contiene imágenes oficiales (nginx, mysql, python, node, etc.) y de la comunidad.

Registros privados: para organizaciones, pueden usar Docker Trusted Registry, Harbor, GitLab Container Registry, AWS ECR, etc.

Comandos relacionados:

```
docker pull <imagen>          # descargar imagen
docker push <imagen>          # subir imagen a un registro
docker tag <origen> <destino> # etiquetar imagen (para asociarla a un repositorio)
```

## 8. Ejercicio de reflexión
Para asegurar que entendiste los conceptos, responde las siguientes preguntas sin usar comandos:

¿Cuál es la diferencia principal entre una imagen y un contenedor?

¿Por qué Docker es más ligero que una máquina virtual?

Explica brevemente qué son los namespaces y los cgroups.

¿Qué es una capa (layer) en una imagen Docker y por qué es útil?

¿Qué papel juega el Docker Daemon en la arquitectura?

Respuestas sugeridas (puedes ocultarlas con <details> para que el estudiante no las vea de inmediato):

<details> <summary>Ver respuestas</summary>
La imagen es una plantilla de solo lectura; el contenedor es una instancia en ejecución de esa imagen con una capa de escritura adicional.

Porque comparte el kernel del host y no incluye un sistema operativo completo; los procesos están aislados mediante namespaces.

Los namespaces aíslan recursos del sistema (procesos, red, montajes) para cada contenedor; los cgroups limitan y controlan los recursos (CPU, memoria) que puede usar.

Una capa es una modificación sobre la imagen base (por ejemplo, instalar un paquete). Permiten reutilización y descargas incrementales.

Es el proceso en segundo plano que gestiona los contenedores, imágenes, redes y volúmenes, y recibe órdenes del cliente Docker.

</details> 