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
