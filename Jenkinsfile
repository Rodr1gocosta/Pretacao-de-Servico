pipeline {
    agent any
    tools {
        nodejs "Node v14.20"
    }
    environment {
            DOCKER_IMAGE_GATEWAY = "rodr1gocosta/gateway-image"
            DOCKER_IMAGE_FRONTEND = "rodr1gocosta/frontend-image"
    }
    stages {
        stage('Checkout') {
            steps {
                    checkout scm
                    sh 'git submodule update --init --recursive'
            }
        }
        stage('Install Dependencies') {
            steps {
                // Instalar dependências dos componentes
                dir('Gateway') {
                    // sh '' não precisa no Gateway
                }
                dir('front-end-prestacao-de-servico') {
                    sh 'npm install'
                }
            }
        }
        stage('Build Gateway') {
            steps {
                dir('Gateway') {
                    sh 'mvn clean'
                    sh 'mvn clean package'
                }
            }
        }
        stage('Build Frontend') {
            steps {
                dir('front-end-prestacao-de-servico') {
                    sh 'npm run build'
                }
            }
        }
        stage('Test') {
            steps {
                dir('Gateway') {
                    sh 'mvn test'
                }
                dir('front-end-prestacao-de-servico') {
                    // sh 'npm test'
                }
            }
        }
        stage('Criar imagem Docker') {
            steps {
                script {
                    dir('Gateway') {
                        dockerapp = docker.build("${DOCKER_IMAGE_GATEWAY}:${env.BUILD_ID}", '-f Dockerfile .')
                    }
                    dir('front-end-prestacao-de-servico') {
                        dockerapp = docker.build("${DOCKER_IMAGE_FRONTEND}:${env.BUILD_ID}", '-f Dockerfile .')
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed!'
        }
    }
}
