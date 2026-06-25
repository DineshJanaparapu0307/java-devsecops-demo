pipeline {
agent any

```
environment {
    IMAGE_NAME = "dineshjanaparapu/devsecops-demo"
}

stages {

    stage('Build') {
        steps {
            sh 'chmod +x mvnw'
            sh './mvnw clean compile'
        }
    }

    stage('Unit Test') {
        steps {
            sh './mvnw test'
        }
    }

    stage('SonarQube Analysis') {
        steps {
            withSonarQubeEnv('Sonar') {
                sh '''
                ./mvnw sonar:sonar \
                -Dsonar.projectKey=java-devsecops-demo \
                -Dsonar.host.url=http://localhost:9000
                '''
            }
        }
    }

    stage('Trivy Filesystem Scan') {
        steps {
            sh 'trivy fs .'
        }
    }

    stage('Package') {
        steps {
            sh './mvnw package -DskipTests'
        }
    }

    stage('Upload Artifact to Nexus') {
        steps {
            sh './mvnw deploy -DskipTests'
        }
    }

    stage('Docker Build') {
        steps {
            sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
        }
    }

    stage('Trivy Image Scan') {
        steps {
            sh 'trivy image $IMAGE_NAME:$BUILD_NUMBER'
        }
    }

    stage('Push Docker Image') {
        steps {
            withCredentials([usernamePassword(
                credentialsId: 'dockerhub-creds',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS'
            )]) {
                sh '''
                echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                docker push $IMAGE_NAME:$BUILD_NUMBER
                '''
            }
        }
    }
}
```

}

