pipeline {
    agent any
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World!'
            }
        }
        stage('Check node version') {
            steps {
                bat "node --version"
            }
        }
    }
}