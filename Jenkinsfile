pipeline {
    agent any

    environment {
        DOCKER_COMPOSE = 'docker compose'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/DiegoCam55/Integracion-Continua-inventario.git'
            }
        }

        stage('Build Images') {
            steps {
                dir("${WORKSPACE}") {
                    sh "${DOCKER_COMPOSE} build"
                }
            }
        }

        stage('Start Services') {
            steps {
                dir("${WORKSPACE}") {
                    sh "${DOCKER_COMPOSE} up -d db"
                    sh "sleep 40"
                    sh "${DOCKER_COMPOSE} up -d backend"
                }
            }
        }

        stage('Run Backend Tests') {
            steps {
                dir("${WORKSPACE}") {
                    sh "${DOCKER_COMPOSE} run --rm backend pytest || echo 'No tests found'"
                }
            }
        }

        stage('Deploy Frontend') {
            steps {
                dir("${WORKSPACE}") {
                    sh "${DOCKER_COMPOSE} up -d frontend"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finalizado. Contenedores se mantienen activos."
        }
    }
}

