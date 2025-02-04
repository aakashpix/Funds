pipeline {
    agent any
    environment {
        SONAR_TOKEN = credentials('sonar-token')
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/aakashpix/Funds'
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Run Unit Tests') {
            steps {
                sh 'mvn test'
                junit '**/target/surefire-reports/*.xml'
            }
        }
        
        stage('Code Quality Check') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login=$SONAR_TOKEN'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t funds-transfer-app .'
            }
        }
        
        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub', url: 'https://index.docker.io/v1/']) {
                    sh 'docker tag funds-transfer-app your-docker-username/funds-transfer-app:latest'
                    sh 'docker push your-docker-username/funds-transfer-app:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline execution completed'
        }
        success {
            mail to: 'your-email@example.com',
                subject: "Jenkins Build Successful",
                body: "The Jenkins pipeline executed successfully!"
        }
        failure {
            mail to: 'your-email@example.com',
                subject: "Jenkins Build Failed",
                body: "The Jenkins pipeline failed. Check logs for more details."
        }
    }
}
