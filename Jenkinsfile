pipeline {
    agent any

    parameters {
        string(name: 'FIRST_NAME', defaultValue: 'Bhargav', description: 'Enter your first name')
        string(name: 'LAST_NAME', defaultValue: 'Vallamkonda', description: 'Enter your last name')
        string(name: 'EMAIL', defaultValue: 'bhargav@example.com', description: 'Enter your email address')
        string(name: 'TICKETS', defaultValue: '2', description: 'Enter number of tickets to book')
    }

    environment {
        DB_USER = 'root'
        DB_PASS = '123Nani321@'
        DB_NAME = 'ticketdb'
        DB_HOST = '127.0.0.1'
        DB_PORT = '3306'
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📦 Cloning Repository...'
                checkout scm
            }
        }

        stage('Install Go') {
            steps {
                echo '🧰 Checking and installing Go...'
                sh 'go version || sudo apt update && sudo apt install -y golang-go'
            }
        }

        stage('Build') {
            steps {
                echo '🔨 Building Go Application...'
                sh 'go mod tidy || echo "No go.mod file found, skipping tidy."'
                sh 'go build -o bookingApp main.go'
            }
        }

        stage('Run Application') {
            steps {
                echo "🚀 Running Ticket Booking App for ${params.FIRST_NAME} ${params.LAST_NAME}"
                sh """
                    ./bookingApp <<EOF
                    ${params.FIRST_NAME}
                    ${params.LAST_NAME}
                    ${params.EMAIL}
                    ${params.TICKETS}
                    EOF
                """
            }
        }
    }

    post {
        success {
            echo '✅ Booking successfully added to MySQL Database!'
        }
        failure {
            echo '❌ Pipeline failed. Check console output for details.'
        }
    }
}
