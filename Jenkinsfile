pipeline {
    agent any
    environment {
        // Define DockerHub credentials and repository details
        DOCKERHUB_CREDENTIALS = 'Docker'  // ID of your Docker credentials in Jenkins
        IMAGE_NAME_NGINX = '3booda24/devops-project-django_nginx:latest'
        IMAGE_NAME_DJANGO = '3booda24/devops-project-django_web:latest'
    }
    stages {
        stage('Clone repository') {
            steps {
                // Clones your GitHub repository
                git 'https://github.com/abdelrahmanonline4/DevOps-Project-django'
            }
        }
        stage('Build Django Docker image') {
            steps {
                script {
                    // Builds Docker image for Django app
                    docker.build("${IMAGE_NAME_DJANGO}", '.')
                }
            }
        }
        stage('Build Nginx Docker image') {
            steps {
                script {
                    // Change directory to 'nginx' and build Docker image for Nginx
                    dir('nginx') {
                        docker.build("${IMAGE_NAME_NGINX}", '.')
                    }
                }
            }
        }
        stage('Push Docker Images') {
            steps {
                script {
                    // Logs in to DockerHub and pushes both images
                    docker.withRegistry('', "${DOCKERHUB_CREDENTIALS}") {
                        docker.image("${IMAGE_NAME_DJANGO}").push()
                        docker.image("${IMAGE_NAME_NGINX}").push()
                    }
                }
            }
        }
        stage('Deploy Kubernetes Configs') {
            steps {
                script {
                    // Apply all Kubernetes configurations in the root directory
                    sh 'kubectl apply -f static-pv-pvc.yaml'
                    sh 'kubectl apply -f deploy/'
                }
            }
        }
    }
    post {
        always {
            // Cleanup after the pipeline execution
            cleanWs()
        }
    }
}

