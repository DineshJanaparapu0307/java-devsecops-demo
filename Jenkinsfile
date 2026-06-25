pipeline {
    agent any

    stages {
        stage('Build') {
    steps {
        sh 'chmod +x mvnw'
        sh './mvnw clean package'
    }
}
    stage('SonarQube Analysis') {
    steps {
        withSonarQubeEnv('Sonar') {
            sh '''
            ./mvnw sonar:sonar \
            -Dsonar.projectKey=java-devsecops-demo \
            -Dsonar.host.url=http://localhost:9000 \
            '''
        }
    }
}

        stage('Docker Build') {
            steps {
                sh 'docker build -t devsecops-demo .'
            }
        }
    }
}
