pipeline {
    agent any

    stages {
        stage('Git checkout') {
            steps {
                git branch: 'main', credentialsId: 'GitHub', url: 'https://github.com/Ndejito/sonarqube-nexusRepo.git'
            }
        }
        
        stage('Build with maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        
             stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        
            }
        stage('Code Qualty Scan') {

           steps {
                  withSonarQubeEnv('sonarserver') {
             sh "mvn -f SampleWebApp/pom.xml sonar:sonar"      
               }
            }
       }
        stage('Quality Gate') {
          steps {
                 waitForQualityGate abortPipeline: true
              }
        }
        stage('deploy to tomcat') {
          steps {
              deploy adapters: [tomcat9(credentialsId: 'Tomcat', path: '', url: 'http://18.224.66.164:8080/')], contextPath: '**/*.war', war: ''
              
          }
            
        }
            
        }
}  
 
