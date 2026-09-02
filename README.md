# 🐳 Docker Learning Path

> Una guía completa y práctica para aprender Docker desde cero hasta producción.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)](https://www.docker.com/)

## 📖 ¿Qué es esto?

Este repositorio es una guía estructurada para aprender Docker de forma práctica. 
Cada sección incluye teoría esencial, ejemplos de código funcionales y ejercicios 
prácticos para consolidar el aprendizaje.

## 🎯 Objetivos

- Entender los conceptos fundamentales de Docker
- Crear imágenes propias con Dockerfiles
- Manejar persistencia de datos con volúmenes
- Orquestar múltiples servicios con Docker Compose
- Aplicar buenas prácticas de optimización

## 🗺️ Roadmap de Aprendizaje

### 📚 Nivel Básico
1. [Instalación y configuración](docs/01-instalacion.md)
2. [Primeros pasos y comandos esenciales](docs/02-primeros-pasos.md)

### 🛠️ Nivel Intermedio
3. [Creando mi primer Dockerfile](docs/03-dockerfile.md)
4. [Volúmenes y persistencia de datos](docs/04-volumenes.md)

### 🚀 Nivel Avanzado
5. [Docker Compose para multi-servicios](docs/05-docker-compose.md)
6. [Optimización y buenas prácticas](docs/06-optimizacion.md)

## 📂 Estructura del Repositorio

\`\`\`
docker-learning-path/
├── docs/          # Documentación detallada
├── examples/      # Ejemplos de código por tema
├── scripts/       # Scripts utilitarios
└── .github/       # Configuración de GitHub
\`\`\`

## 🚀 Inicio Rápido

### Requisitos previos
- [Docker Desktop](https://www.docker.com/products/docker-desktop) instalado
- Conocimientos básicos de terminal

### Primeros pasos
\`\`\`bash
# Clonar el repositorio
git clone https://github.com/tu-usuario/docker-learning-path.git
cd docker-learning-path

# Ejecutar el primer ejemplo
cd examples/01-basico
docker build -t mi-primera-app .
docker run -p 3000:3000 mi-primera-app

# Abrir en el navegador
# http://localhost:3000
\`\`\`

## 📚 Documentación Detallada

| Sección | Descripción | Ejemplo de Código |
|---------|-------------|-------------------|
| [01 - Instalación](docs/01-instalacion.md) | Cómo instalar Docker en tu SO | - |
| [02 - Primeros Pasos](docs/02-primeros-pasos.md) | Comandos básicos y ciclo de vida | [Ver ejemplo](examples/01-basico/) |
| [03 - Dockerfile](docs/03-dockerfile.md) | Crear imágenes personalizadas | [Ver ejemplo](examples/02-dockerfile/) |
| [04 - Volúmenes](docs/04-volumenes.md) | Persistencia de datos | [Ver ejemplo](examples/03-volumenes/) |
| [05 - Docker Compose](docs/05-docker-compose.md) | Multi-contenedor | [Ver ejemplo](examples/04-compose/) |
| [06 - Optimización](docs/06-optimizacion.md) | Mejores prácticas | [Ver ejemplo](examples/05-proyecto-final/) |

## 🎓 ¿Cómo usar este repositorio?

1. **Lee la documentación** en orden (empezando por `docs/01-instalacion.md`)
2. **Clona los ejemplos** y ejecútalos en tu máquina
3. **Practica modificando** el código de los ejemplos
4. **Crea tu propio proyecto** basándote en lo aprendido
5. **Comparte tu experiencia** en la sección de Issues/Discussions

## 🤝 Contribuciones

¡Las contribuciones son bienvenidas! Si encuentras un error o quieres mejorar algo:

1. Fork del repositorio
2. Crea una rama (`git checkout -b feature/mejora`)
3. Commit de tus cambios (`git commit -m 'Agrega mejora'`)
4. Push a la rama (`git push origin feature/mejora`)
5. Abre un Pull Request

Lee [CONTRIBUTING.md](CONTRIBUTING.md) para más detalles.

## 📝 Licencia

Este proyecto está bajo la Licencia MIT - ver el archivo [LICENSE](LICENSE) para más detalles.

## ⭐ Apoya el proyecto

Si este repositorio te fue útil:
- Dale una ⭐ en GitHub
- Compártelo en redes sociales
- Menciona el proyecto en tu blog o canal

## 📞 Contacto

- GitHub: [@tu-usuario](https://github.com/tu-usuario)
- Twitter/X: [@tu-usuario](https://twitter.com/tu-usuario)
- LinkedIn: [Tu Nombre](https://linkedin.com/in/tu-perfil)

---

**Happy Coding! 🐳**
