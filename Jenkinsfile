pipeline {
    agent any

    environment {
        REGISTRY = "docker.io/yourdockerhubusername"
        APP = "jenkins-k8s-demo"
        DOCKER_CRED = "docker-hub-cred"    // Jenkins credentials id
        KUBE_CRED = "kubeconfig-cred"      // Jenkins credential holding kubeconfig file
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install & Test') {
            steps {
                dir('app') {
                    sh 'npm install'
                    sh 'npm test'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def tag = "${env.BUILD_NUMBER}"
                    dockerImage = docker.build("${REGISTRY}/${APP}:${tag}")
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CRED) {
                        dockerImage.push("${env.BUILD_NUMBER}")
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                  withCredentials([file(credentialsId: KUBE_CRED, variable: 'KUBECONFIG')]) {
                      sh "sed -i 's#\\${BUILD_TAG}#${env.BUILD_NUMBER}#g' k8s/deployment.yaml"
                      sh "kubectl apply -f k8s/deployment.yaml"
                      sh "kubectl apply -f k8s/service.yaml"
                  }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment succeeded!"
        }
        failure {
            echo "❌ Deployment failed!"
        }
    }
}
