pipeline {
    agent any
    
    tools {
        maven 'localMaven'
    }
    
    stages {
        
        stage('Checkout') {
            steps {
                echo "-=- Checkout project -=-"
                git url: 'https://github.com/jgrisel/shopizer.git'
            }
        }
        
        stage('Package') {
            steps {
                echo "-=- packaging project -=-"
                sh 'mvn clean package -Dmaven.test.skip=true'
            }
            
        }

        stage('Test') {
            steps {
                echo "-=- Test project -=-"
                sh 'JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre mvn clean test'
            }
            
         post {
                success {
                    junit 'sm-core/target/surefire-reports/*.xml'
                }
            } 
   
        } 
        stage('Sonarqube Scanner') {
            environment {
                SCANNER_HOME = tool 'sonarqube'
                ORGANIZATION = "EQL"
                PROJECT_NAME = "Shopizer"
            }
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.java.sources=**/src \
                    -Dsonar.java.binaries=**/target \
                    -Dsonar.projectKey=$PROJECT_NAME \
                    -Dsonar.language=java \
                    -Dsonar.sourceEncoding=UTF-8'''
                  }
            }
        }
      
    }
}
