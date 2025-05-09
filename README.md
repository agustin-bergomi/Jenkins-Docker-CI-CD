# Jenkins-Docker-CI-CD
Example of implementacion - PoC of CI-CD with Docker and Jenkins

# Setup de Jenkins para Automatización Mobile con Docker, Appium, TypeScript y WebDriverIO

Este repositorio explica algunos pasos basicos para un PoC de utilizacion de **Jenkins** localmente usando Docker para poder aplicar Jenkins en un proyecto de Appium usando **Docker** y así correr proyectos de automatización mobile utilizando **Appium**, **TypeScript** y **WebDriverIO**.

## 🛠 Requisitos Previos

- Tener instalado **Docker** y **Docker Compose** en tu máquina.
- Tener instalado **Node.js** y **npm**.
- Tener acceso a las imágenes de Jenkins (docker hub).
- Conocimientos básicos de automatización móvil y WebDriverIO.

---

## 📦 Instalación de Jenkins

1. **Levantar Jenkins con Docker**
   
   Creamos un contenedor de Jenkins utilizando Docker:

   ```
   docker run -d \
     -p 8080:8080 -p 50000:50000 \
     -v jenkins_home:/var/jenkins_home \
     --name jenkins-server \
     jenkins/jenkins:lts

## Instalacion de Plugins basicos
<img width="994" alt="Screenshot 2025-04-28 at 12 30 05 PM" src="https://github.com/user-attachments/assets/d22aca3d-e0b5-4006-95f9-52d7f5436834" />

🧩 Plugins Recomendados para Integración con Docker, Appium y WebDriverIO

- Docker Pipeline Plugin
- NodeJS Plugin
- Git Plugin
- Pipeline Plugin
- Blue Ocean Plugin (opcional, para mejor visualización)

```
 pipeline {
    agent any

    stages {
        stage('Test Docker Access') {
            steps {
                sh 'docker ps'
            }
        }
    }
}
```


## Creacion de pipeline para verificar status de la configuracion
<img width="1198" alt="Screenshot 2025-04-28 at 7 51 27 PM" src="https://github.com/user-attachments/assets/686c75de-3f15-4db3-9069-0b8f8642f312" />

Ejecucion del pipeline existosa: Jenkins puede usar Docker correctamente
<img width="923" alt="Screenshot 2025-04-28 at 7 54 43 PM" src="https://github.com/user-attachments/assets/4acf7b64-38c7-4762-9edf-616ab448db5f" />


## Si falla el pipelines se puede crear un Dockerfile
que prepare Jenkins listo para
- Usar Node.js (para ejecutar npm install y WebDriverIO).
- Usar Docker CLI (para levantar el servidor de Appium dentro de los pipelines).

Dockerfile utilizado dio el sigguiente error:

--------------------
ERROR: failed to solve: process "/bin/sh -c apt-get update &&     apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common &&     curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - &&     add-apt-repository \"deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable\" &&     apt-get update &&     apt-get install -y docker-ce-cli" did not complete successfully: exit code: 100


significa que el paquete docker-ce-cli no está disponible para la distribución Debian Bookworm (la que usa Jenkins LTS actual internamente).


**Dockerfile actualizado:**

```
FROM jenkins/jenkins:lts

USER root

# Instalar herramientas básicas y Docker CLI (docker.io)
RUN apt-get update && \
    apt-get install -y apt-transport-https ca-certificates curl gnupg2 software-properties-common lsb-release && \
    apt-get install -y docker.io

# Instalar Node.js y npm (versión 18 LTS)
RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && \
    apt-get install -y nodejs

# Verificar versiones instaladas (opcional)
RUN node -v && npm -v && docker --version

USER jenkins
```
