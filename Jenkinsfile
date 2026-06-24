pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh './mvnw clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t devsecops-demo .'
            }
        }
    }
}