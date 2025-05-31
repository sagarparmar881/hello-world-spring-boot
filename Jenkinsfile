pipeline {
    agent any

    environment {
        IMAGE_NAME = 'springboot-local'
        CONTAINER_NAME = 'springboot-dev'
        PORT = '8090'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git url: 'https://github.com/sagarparmar881/hello-world-spring-boot.git', branch: 'main'
            }
        }

        stage('Build App') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                    docker run -d -p $PORT:$PORT --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo "App deployed locally at http://application.dev:$PORT"
        }
        failure {
            echo "Build failed."
        }
    }
}
