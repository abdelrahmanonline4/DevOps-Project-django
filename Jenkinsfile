pipeline {
    agent any

    environment {
        IMAGE_NAME_NGINX = '3booda24/devops-project-django_nginx:latest'
        IMAGE_NAME_DJANGO = '3booda24/devops-project-django_web:latest'
    }

    stages {
        stage('Build Django Docker Image') {
            steps {
                script {
                    def app = docker.build(IMAGE_NAME_DJANGO)
                }
            }
        }

        stage('Build Nginx Docker Image') {
            steps {
                script {
                    dir('nginx') {
                        def nginx = docker.build(IMAGE_NAME_NGINX)
                    }
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-3booda24') {
                        sh "docker login -u 3booda24 -p bedomm180Ab"
                        docker.image(IMAGE_NAME_DJANGO).push()
                        docker.image(IMAGE_NAME_NGINX).push()
                    }
                }
            }
        }

        stage('Deploy Kubernetes Configurations') {
            steps {
                script {
                    
                    sh 'kubectl apply -f . '
                    sh 'kubectl apply -f deploy/'

                }
            }
        }
    }
}
