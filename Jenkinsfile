pipeline {
    agent any

    environment {
        IMAGE_NAME = "laravel-three-tier-app"
        CONTAINER_NAME = "laravel_app_instance"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
                }
            }
        }

        stage('Run Container & Verify Database Connectivity') {
            steps {
                script {
                    // Stop and remove old container if it exists
                    sh "docker rm -f ${CONTAINER_NAME} || true"
                    
                    // Run the container and link it to your running mysql-db-container via Docker network or host bridge
                    sh """
                        docker run -d \
                          --name ${CONTAINER_NAME} \
                          -p 8000:8000 \
                          -e DB_CONNECTION=mysql \
                          -e DB_HOST=172.17.0.1 \
                          -e DB_PORT=3306 \
                          -e DB_DATABASE=laravel_db \
                          -e DB_USERNAME=root \
                          -e DB_PASSWORD=secret \
                          ${IMAGE_NAME}:${BUILD_NUMBER}
                    """
                }
            }
        }
    }
}
