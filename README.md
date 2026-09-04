# 🐳 Docker Learning

![Docker Logo](https://raw.githubusercontent.com/docker-library/docs/master/docker/logo.png)

[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://docs.docker.com/)
[![Docker Compose v2](https://img.shields.io/badge/Docker%20Compose%20v2-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://docs.docker.com/compose/)
[![Licencia MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

## 🗺️ Ruta de Aprendizaje

El contenido está distribuido en **4 fases progresivas**. Cada etapa construye los cimientos de la siguiente:

| Fase | Módulo | ¿Qué aprenderás? | Acceso directo |
| :---: | :--- | :--- | :---: |
| **01** | **Comandos Esenciales** | Gestión integral de imágenes, contenedores, inspección de procesos y logs en tiempo real. | [💻 Ver módulo](01-Commands/README.md) |
| **02** | **Imágenes y Dockerfiles** | Sintaxis moderna de `Dockerfile`, técnicas de *Multi-stage build* y optimización de caché con Buildx. | [🧱 Ver módulo](02-Dockerfile-images/README.md) |
| **03** | **Persistencia y Redes** | Volúmenes nombrados, *bind mounts*, drivers de red tipo bridge y aislamiento entre contenedores. | [💾 Ver módulo](03-Volumes-and-networks/README.md) |
| **04** | **Orquestación Local** | Definición de stacks completos (Backend + DB + Frontend) mediante archivos `docker-compose.yml`. | [🚀 Ver módulo](04-Docker-Compose/README.md) |

---

## 📁 Estructura del Repositorio

El proyecto mantiene una estructura modular y limpia para facilitar tu estudio:

```text
.
├── 📁 Configuracion/            # Guías de instalación (Linux, macOS, Windows WSL 2)
│   ├── Linux/
│   ├── MacOS/
│   └── Windows/
├── 📁  Documentacion/              # Arquitectura y fundamentos teóricos
│   └── examples/                   # Diagramas y referencias
├── 📁 01 Commands/              # Prácticas con CLI de Docker
├── 📁 02 Dockerfile images/     # Construcción y optimización de imágenes
├── 📁 03 Volumes and networks/  # Persistencia de datos y redes virtuales
├── 📁 04 Docker Compose/        # Stacks multi-contenedor listos para producción
├── LICENSE                     # Licencia de código abierto
└── README.md                   # Índice principal del curso
```

## 📚 Documentación y Recursos Oficiales

- 📖 [Documentación Oficial de Docker](https://docs.docker.com/)
- 🧱 [Guía de Referencia de Dockerfile](https://docs.docker.com/reference/dockerfile/)
- 🚀 [Especificación de Docker Compose](https://docs.docker.com/compose/)
- 📦 [Docker Hub Registry](https://hub.docker.com/)

## 🤝 Contribuciones y Comunidad

¡Las contribuciones son bienvenidas! Si encuentras algún comando desactualizado, un error tipográfico o quieres proponer un nuevo reto práctico:

1. Haz un **Fork** de este repositorio.
2. Crea tu rama de características (`git checkout -b feature/mejora-modulo`).
3. Realiza tus commits (`git commit -m 'feat: agrega ejemplo práctico de multi-stage'`).
4. Sube los cambios (`git push origin feature/mejora-modulo`).
5. Abre un **Pull Request**.

## Licencia

Distribuido bajo la [LICENSE](LICENSE). Si este material te resulta útil, ¡no olvides darle una ⭐ al repositorio!
