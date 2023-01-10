pipeline {
    agent {
                label 'Shopizer'
            }
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
                sh 'mvn clean install -Dmaven.test.skip=true'
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
        
        stage('Continuous deployment') {
          steps {
               echo "-=- Deployment -=-"
               sh 'cd sm-shop sudo JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre mvn spring-boot:run' 
             }
          }
        }
}
