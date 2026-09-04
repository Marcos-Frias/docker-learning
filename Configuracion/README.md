# Configurar Docker

Antes de comenzar las lecciones, prepara Docker correctamente en tu sistema operativo. Esta carpeta reúne las guías de instalación, configuración y comprobación para las plataformas principales.

>[!TIP]
> Elige la guía que corresponda a tu sistema. No necesitas leer las tres.

## Selecciona tu sistema operativo

| Sistema operativo | Guía | Método recomendado |
| --- | --- | --- |
| Windows 10/11 | [Configurar Docker en Windows](Windows/README.md) | Docker Desktop con WSL 2 |
| macOS | [Configurar Docker en macOS](MacOS/README.md) | Docker Desktop |
| Linux | [Configurar Docker en Linux](Linux/README.md) | Docker Engine |

## Qué aprenderás

Cada guía incluye:

- Requisitos previos del sistema.
- Instalación de Docker.
- Configuración necesaria para trabajar con contenedores.
- Comandos para comprobar que Docker funciona.
- Solución para los problemas más frecuentes.

> [!NOTE]
> Las pantallas y algunos comandos pueden variar según la versión de tu sistema operativo. Si una opción cambia de nombre, utiliza la documentación oficial enlazada al final de esta página.

## Comprobación rápida

Cuando termines la guía de tu sistema operativo, ejecuta:

```bash
docker --version
docker compose version
docker run hello-world
```

Si el contenedor `hello-world` muestra el mensaje de bienvenida, Docker está listo para utilizarse.

> [!IMPORTANT]
> Completa primero la guía de tu sistema operativo. Docker Desktop debe estar abierto en Windows y macOS; en Linux, el servicio de Docker debe estar activo.

## Importante para Windows

Docker Desktop en Windows funciona mejor utilizando el motor basado en **WSL 2**. La guía de Windows explica cómo instalar WSL 2, actualizarlo y activar su integración con Docker Desktop.

## Documentación oficial

- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Instalación de Docker Engine](https://docs.docker.com/engine/install/)
- [Documentación de WSL](https://learn.microsoft.com/es-es/windows/wsl/)
- [Referencia de Docker Compose](https://docs.docker.com/compose/)
