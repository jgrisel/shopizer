pipeline {
    agent none
    tools {
        maven 'localMaven'
    }   
    stages {
        
        stage('Checkout') {
                 agent {
                label 'built-in'
            }
            steps {
                echo "-=- Checkout project -=-"
                git url: 'https://github.com/jgrisel/shopizer.git'
            }
        }
        
        stage('Package') {
                        agent {
                label 'built-in'
            }
            steps {
                echo "-=- packaging project -=-"
                sh 'mvn clean install -Dmaven.test.skip=true'
            }
            
        }
            
        stage('Test') { 
                        agent {
                label 'built-in'
            }
            steps {
                echo "-=- Test project -=-"
                sh 'JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre mvn clean test'
                junit 'sm-core/target/surefire-reports/*.xml'
                }
            }
        
      stage('Selenium Test Job') {
            steps {
                 build job: 'test2' 
            }
        }

  
        stage('Sonarqube Scanner') {
                        agent {
                label 'built-in'
            }
            steps {
                echo "-=- Analyse Project -=-"
                sh 'JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre mvn clean verify sonar:sonar -Dsonar.projectKey=shopizer -Dsonar.host.url=http://192.168.102.181:9000 -Dsonar.login=d6500b8b1dfdb5511016df4b0238824d3ca0d05c'
                  }
            }
  }
}

