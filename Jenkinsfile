pipeline {
    agent any
    environment{
        PATH = "$PATH:/usr/share/maven/bin"
    }

    stages {
         stage('Cloning from GitHub') {
                steps {
                    git branch: 'main', url: 'https://github.com/ihebmbarki/Devops-Spring.git'
                }
                
            }
          stage('MVN COMPILE') {
            steps {
               sh 'mvn compile'
            
           }
        }
        stage('MVN CLEAN') {
            steps {
               sh 'mvn clean install'
            }
        }
          stage('Test') {
            steps {
               sh 'mvn test'
            }
        }
          stage('Test package') {
            steps {
               sh 'mvn package'
            }
        }
      
      
        stage ('Scan Sonar'){
            steps {
        
    sh "mvn sonar:sonar \
  -Dsonar.projectKey=sonar \
  -Dsonar.host.url=http://192.168.33.10:9000 \
  -Dsonar.login=c1b1c39f0ba6e01cbb9c403e6eb6697f76a807bf"

    }
        }
        stage('Nexus') {
      steps {
        sh 'mvn deploy -DskipTests'
      }
    }
       
     stage("Building Docker Image") {
                steps{
                    sh 'docker build -t ihebmbarki7/achat .'
                }
        }
        
        
           stage("Login to DockerHub") {
                steps{
                   // sh 'sudo chmod 666 /var/run/docker.sock'
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u ihebmbarki7 -p @P4r4lys!s09'
                }
        }
        stage("Push to DockerHub") {
                steps{
                    sh 'docker push ihebmbarki7/achat'
                }
        }
    
               stage("Docker-compose") {
                steps{
                    sh 'docker-compose up -d'
                }
        }
    }
    
    post {
                success {
                   echo 'succes'
                }
failure {
                  echo 'failed'   
                }
             
       
    }

}
