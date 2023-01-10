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
                sh 'scp /var/lib/jenkins/workspace/Shopizer_Pipeline/sm-shop/target/ROOT.war shopizer@192.168.102.169:/home/shopizer/shopizer/sm-shop/target'
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
            agent {
                label 'Shopizer'
            }
          steps {
               echo "-=- Deployment -=-"
               sh 'cd shopizer/sm-shop'
               sh 'mvn spring-boot:run' 
             }
          }
        
        stage('Sonarqube Scanner') {
            steps {
                echo "-=- Analyse Project -=-"
                sh 'JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre mvn clean verify sonar:sonar -Dsonar.projectKey=Shopizer -Dsonar.host.url=http://192.168.102.125:9000 -Dsonar.login=7a47331db205535b3f724ae8c8bc088e6a041a17'
                  }
            }
        }
}
