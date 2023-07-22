pipeline {
    agent any
    tools {
    maven 'Maven' 
       }

    stages {
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
              deploy adapters: [tomcat9(credentialsId: 'Tomcat', path: '', url: 'http://18.224.66.164:8080/')], contextPath: 'myapp/', war: '**/*.war'
              
          }
            
        }
            
        }
}  
 
