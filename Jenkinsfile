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
            steps {
                script {
                    // Build Docker image using Jenkins Docker Pipeline API
                    echo "Building Docker image ${IMAGE_NAME}..."
                    app = docker.build("${IMAGE_NAME}")
                    app.tag("latest")
                }
            }
        }

        stage('Snyk Scan') {
            steps {
                script {
                    echo "Running Snyk scan..."
                    sh "snyk test --all-projects"
                    // Optional: sh "snyk monitor --all-projects" to send results to Snyk dashboard
                }
            }
        }
        
        stage('POST-TO-DOCKERHUB') {   
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



    }  
} 
