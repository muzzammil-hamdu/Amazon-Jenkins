pipeline {
    agent any
    environment {
        // Use PATH+EXTRA to append to PATH properly
        PATH = "/usr/bin:/bin:/opt/homebrew/bin"
    }
    stages {
        stage('pull') {
            steps {
                git branch: 'main', url: 'https://github.com/PraveenKuber/Amazon-Jenkins.git'
            }
        }

        stage('compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('LocalSonarQube') {
                    sh 'mvn sonar:sonar'
                }
                // Debug: show task ID and project details
                sh 'echo "===== Sonar Scanner Report-Task.txt ====="'
                sh 'cat target/sonar/report-task.txt || cat .scannerwork/report-task.txt || true'
            }
        }
    }

    post {
        success {
            echo 'Build success'
        }
        failure {
            echo 'Failure in the build'
        }
    }
}
