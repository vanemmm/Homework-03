pipeline {
    agent any

    environment {
        // Docker Hub credentials ID stored in Jenkins
        DOCKERHUB_CREDENTIALS ='cyber-3120'
        IMAGE_NAME ='vanemm/gametest123'
    }

    stages {

        stage('Cloning Git') {
            steps {
                checkout scm
            }
        }

      stage('BUILD-AND-TAG') {
            agent {
                label 'agent001'
            }
            steps {
                script {
                    // Build Docker image using Jenkins Docker Pipeline API
                    echo "Building Docker image ${IMAGE_NAME}..."
                    app = docker.build("${IMAGE_NAME}")
                    app.tag("latest")
                }
            }
        }


        stage('POST-TO-DOCKERHUB') {    
            agent {
                label 'agent001'
            }
            steps {
                script {
                    echo "Pushing image ${IMAGE_NAME}:latest to Docker Hub..."
                    docker.withRegistry('https://registry.hub.docker.com', "${DOCKERHUB_CREDENTIALS}") {
                        app.push("latest")
                    }
                }
            }
        }


        stage('Pull-image-server') {
            steps {
                sh 'echo Pulling image on server...'
            }
        }


        stage('DEPLOYMENT') {    
            agent {
                label 'agent001'
            }
            steps {
                echo 'Starting deployment using docker-compose...'
                script {
                    dir("${WORKSPACE}") {
                        sh '''
                            docker-compose down
                            docker-compose up -d
                            docker ps
                        '''
                    }
                }
                echo 'Deployment completed successfully!'
            }
        }
    }  
} 