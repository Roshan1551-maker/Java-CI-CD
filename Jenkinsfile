pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub'
        KUBERNETES_CREDENTIALS_ID = 'kubeconfig'
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Roshan1551-maker/Java-CI-CD.git'
            }
        }
        stage('Build Java Application') {
            agent { docker { image 'openjdk:17' } }
            steps {
                sh 'javac App.java'
            }
        }
        stage('Run Unit Tests') {
            agent { docker { image 'openjdk:11' } }
            steps {
                sh 'java -jar tests.jar'
            }
        }
        stage('Analyze Code Quality') {
            agent { docker { image 'sonarqube:latest' } }
            steps {
                sh 'sonar-scanner'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp:latest .'
            }
        }
        stage('Push Image to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: DOCKER_CREDENTIALS_ID]) {
                    sh 'docker push myapp:latest'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig([credentialsId: KUBERNETES_CREDENTIALS_ID]) {
                    sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }
}

