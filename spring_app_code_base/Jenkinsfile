pipeline {
    agent any

    parameters {
        choice(name: 'ENVIRONMENT', choices: ['dev', 'prod'], description: 'Select target environment (dev or prod)')
        choice(name: 'ACTION', choices: ['deploy', 'stop', 'remove'], description: 'Choose action to perform')
    }

    environment {
        SPRING_IMAGE_NAME = "sak-spring-dev"
        SPRING_CONTAINER_NAME = "sak-spring-dev"
        DB_ROOT_PASSWORD = "1234"
        DB_NAME = "mydb"
        DB_PORT = "3306"
        APP_PORT = "8088"
        SPRING_PROFILE = "dev"
    }

    stages {

        stage('Deploy Application Dev Environment') {
            when {
                allOf {
                    expression { params.ENVIRONMENT == 'dev' }
                    expression { params.ACTION == 'deploy' }
                }
            }
            steps {
                echo "🚀 Deploying Spring + MySQL Dev Environment..."
                sh """
                cd ${WORKSPACE}/app_server
                DB_CONTAINER_NAME=${DB_CONTAINER_NAME} \
                SPRING_CONTAINER_NAME=${SPRING_CONTAINER_NAME} \
                SPRING_IMAGE_NAME=${SPRING_IMAGE_NAME} \
                DB_ROOT_PASSWORD=${DB_ROOT_PASSWORD} \
                DB_NAME=${DB_NAME} \
                DB_PORT=${DB_PORT} \
                APP_PORT=${APP_PORT} \
                SPRING_PROFILE=${SPRING_PROFILE} \
                docker-compose up -d --build
                """
            }
        }

        stage('Stop Dev Environment') {
            when {
                allOf {
                    expression { params.ENVIRONMENT == 'dev' }
                    expression { params.ACTION == 'stop' }
                }
            }
            steps {
                echo "🛑 Stopping Spring + MySQL Dev Containers..."
                sh """
                cd ${WORKSPACE}/app_server
                docker-compose stop
                """
            }
        }

        stage('Terminate Dev Environment') {
            when {
                allOf {
                    expression { params.ENVIRONMENT == 'dev' }
                    expression { params.ACTION == 'remove' }
                }
            }
            steps {
                echo "🔥 Removing Dev Environment (containers + volume)..."
                sh """
                cd ${WORKSPACE}/app_server
                docker-compose down -v
                """
            }
        }
    }

    post {
        success {
            echo "✅ Spring Boot environment handled successfully."
        }
        failure {
            echo "❌ Deployment failed. Check logs."
        }
        always {
            echo "ℹ️ Pipeline finished."
        }
    }
}
