pipeline {
    agent any

    environment {
        IMAGE_NAME = "gymuleam-site"
        CONTAINER_NAME = "gymuleam-container"
        HOST_PORT = "8086"
        CONTAINER_PORT = "80"
    }

    tools {
        dockerTool "Dockertool" // asegúrate que en Jenkins esté configurado como 'default'
    }

    stages {
        stage('Clonar Repositorio') {
            steps {
                checkout scm
            }
        }

        stage('Construir Imagen Docker') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Desplegar Contenedor') {
            steps {
                sh '''
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                    docker run -d --name $CONTAINER_NAME -p $HOST_PORT:$CONTAINER_PORT $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Despliegue exitoso en http://localhost:$HOST_PORT"
        }
        failure {
            echo "❌ Falló el despliegue"
        }
    }
}
