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
                  withSonarQubeEnv('SonarServer') {
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
              deploy adapters: [tomcat9(credentialsId: 'Tomcat', path: '', url: 'http://3.129.8.112:8080/')], contextPath: '/myapp', war: '**/*.war'
              
          }
            
        }
            
        }
}  
 
