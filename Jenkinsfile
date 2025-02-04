pipeline {    
    agent any    

    environment {        
        SONAR_TOKEN = credentials('sqa_d2b36428e99350fcd815046ed8c9596564c8202c')    
    }    

    stages {        
        stage('Checkout Code') {            
            steps {                
                git branch: 'main', url: 'https://github.com/aakashpix/Funds.git'            
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
    }    

    post {        
        always {            
            echo 'Pipeline execution completed'        
        }        
        success {            
            mail to: 'your.email@domain.com',                
                subject: "Jenkins Build Successful",                
                body: "The Jenkins pipeline executed successfully!"        
        }        
        failure {            
            mail to: 'your.email@domain.com',                
                subject: "Jenkins Build Failed",                
                body: "The Jenkins pipeline failed. Check logs for more details."        
        }    
    } 
}
