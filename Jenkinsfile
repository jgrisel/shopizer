pipeline {
    agent any
    tools {
        maven 'localMaven'
    }
    environment {
        BUILD_ID = 'dontKillMe'
    }    
    stages {
        stage('Checkout') {
            agent {
                label 'Shopizer'
            }
            steps {
                echo "-=- Checkout project -=-"
                git url: 'https://github.com/jgrisel/shopizer.git'
            }
        }
        
        stage('Package') {
            agent {
                label 'Shopizer'
            }
            steps {
                echo "-=- packaging project -=-"
                sh 'mvn clean install -Dmaven.test.skip=true'
            }
            
        }

        stage('Test') {
            agent {
                label 'Shopizer'
            }
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
              agent {
                label 'Shopizer'
            }
               echo "-=- Deployment -=-"
               sh 'cd sm-shop sudo JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre mvn spring-boot:run'
               echo "-=- Checkout project automation -=-"
             }
          }
        stage('Checkout Selenium') {
            steps {
                echo "-=- Checkout project -=-"
                git url: 'https://github.com/fatimaAmeza/shopiserTest.git'
            }
        }
        
        stage('Selenium Test Job') {
            steps {
                 sh 'chmod +x driver/chromedriver.exe'
                 sh 'mvn clean verify surefire-report:report-only'
                
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
      }
    }
