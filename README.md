# Jenkins-Docker-CI-CD
Example of implementacion - PoC of CI-CD with Docker and Jenkins

# 🚀 Setup de Jenkins para Automatización Mobile con Docker, Appium, TypeScript y WebDriverIO

Este repositorio explica paso a paso cómo instalar y configurar **Jenkins** localmente, para integrarlo con **Docker** y así correr proyectos de automatización mobile utilizando **Appium**, **TypeScript** y **WebDriverIO**.

## 🛠 Requisitos Previos

- Tener instalado **Docker** y **Docker Compose** en tu máquina.
- Tener instalado **Node.js** y **npm**.
- Tener acceso a las imágenes de Jenkins (docker hub).
- Conocimientos básicos de automatización móvil y WebDriverIO.

---

## 📦 Instalación de Jenkins

1. **Levantar Jenkins con Docker**
   
   Creamos un contenedor de Jenkins utilizando Docker:

   ```bash
   docker run -d \
     -p 8080:8080 -p 50000:50000 \
     -v jenkins_home:/var/jenkins_home \
     --name jenkins-server \
     jenkins/jenkins:lts

