pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKER_IMAGE = "pradyumnaragothaman/react-jenkins-docker-k8s"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/pradyumnarago/react-jenkins-docker-k8s.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    // Clean npm cache and remove node_modules and package-lock.json
                    sh 'rm -rf package-lock.json node_modules'
                    sh 'npm cache clean --force'
                    
                    // Install dependencies
                    sh 'npm install'

                    // Build the Docker image
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKER_HUB_CREDENTIALS') {
                        docker.image(DOCKER_IMAGE).push("latest")
                    }
                }
            }
        }
        stage('Deploy with Docker') {
            steps {
                script {
                    // Run Docker commands to deploy the application
                    sh 'docker run -d -p 80:80 pradyumnaragothaman/react-jenkins-docker-k8s'
                }
            }
        }
    }
}
