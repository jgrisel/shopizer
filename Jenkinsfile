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
  
 
      stages {
        stage('Checkout Selenium') {
                agent {
                label 'Windows'
            }
            steps {
                echo "-=- Checkout project -=-"
                git url: 'https://github.com/fatimaAmeza/shopiserTest.git'
            }
        }
        
        stage('Selenium Test Job') {
               agent {
                label 'Windows'
            }
            steps {
                 bat 'mvn clean verify surefire-report:report-only'
                
                publishHTML target: [
            allowMissing: false,
            alwaysLinkToLastBuild: false,
            keepAll: true,
            reportDir: 'target/site',
            reportFiles: 'surefire-report.html',
            reportName: 'Automation Tests Report'
          ]
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
}
