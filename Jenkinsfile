pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "Hakim06/app:latest"  // Remplacez par votre nom d'utilisateur Docker Hub
        KUBE_NAMESPACE = "default"
        REGISTRY_URL = "docker.io"  // Utilisez Docker Hub par défaut
        GITHUB_CREDENTIALS = credentials('git-hub')  // Credentials GitHub
        DOCKER_CREDENTIALS = credentials('Docker')  // Credentials Docker Hub
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                git branch: 'master', credentialsId: 'github-credentials', url: 'https://github.com/votre-username/votre-repo.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${DOCKER_IMAGE} ."
                sh "docker tag ${DOCKER_IMAGE} ${REGISTRY_URL}/${DOCKER_IMAGE}"
                sh "echo ${DOCKER_CREDENTIALS_PSW} | docker login -u ${DOCKER_CREDENTIALS_USR} --password-stdin"
                sh "docker push ${REGISTRY_URL}/${DOCKER_IMAGE}"
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to Kubernetes...'
                sh "kubectl apply -f k8s/deployment.yaml -n ${KUBE_NAMESPACE}"
                sh "kubectl apply -f k8s/service.yaml -n ${KUBE_NAMESPACE}"
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}