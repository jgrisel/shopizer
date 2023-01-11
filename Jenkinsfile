pipeline {
    agent none
    tools {
        maven 'localMaven'
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

        stage('Run Test parallel') {
            
        parallel {
            
        stage('Test') {    
            agent {
                label 'Shopizer'
            }
            steps {
                echo "-=- Test project -=-"
                sh 'JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre mvn clean test'
                junit 'sm-core/target/surefire-reports/*.xml'
                echo "-=- Deployment -=-"
                sh """
                    cd sm-shop
                    sudo JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre JENKINS_NODE_COOKIE=dontKillMe mvn spring-boot:run
                    """
                }
            }
  
 
        stage('Checkout Selenium') {
            steps {
                agent {
                label 'built-in'
            }
                echo "-=- Checkout project -=-"
                git url: 'https://github.com/fatimaAmeza/shopiserTest.git'
            }
        }
        
        stage('Selenium Test Job') {
            steps {
                  agent {
                label 'built-in'
            }
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
        stage('Sonarqube Scanner') {
            steps {
                agent {
                label 'Shopizer'
            }
                echo "-=- Analyse Project -=-"
                sh 'sudo JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre mvn clean verify sonar:sonar -Dsonar.projectKey=Shopizer -Dsonar.host.url=http://192.168.102.181:9000 -Dsonar.login=445562f9d9543ceef67739b2cb1cb0f57fa77399'
                  }
            }
            
        stage('Continuous delivery') {
          steps {
              agent {
                label 'Shopizer'
            }
            sh """
                    sudo mv ROOT.war /home/vagrant/project
                    cd project
                    sudo docker build -t springbootapp1 . 
                    docker tag springbootapp1 jgrisel/springbootapp1:1.0
                    docker push jgrisel/springbootapp1:1.0
                    """ 
            
        }
       
    }
  }
}
