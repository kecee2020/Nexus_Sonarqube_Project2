pipeline {
    agent any

    stages {   
        stage('Build with maven') {
            steps {
                sh 'cd WebApplication && mvn clean install'
            }
        }
        
             stage('Test') {
            steps {
                sh 'cd WebApplication && mvn test'
            }
        
            }
        stage('Code Qualty Scan') {

           steps {
                  withSonarQubeEnv('sonar') {
             sh "mvn -f WebApplication/pom.xml sonar:sonar"      
               }
            }
       }
        stage('Quality Gate') {
          steps {
                 waitForQualityGate abortPipeline: true
              }
        }
        stage('push to nexus') {
            steps {
               nexusArtifactUploader artifacts: [[artifactId: 'WebApplication', classifier: '', file: 'WebApplication/target/WebApplication.war', type: 'war']], credentialsId: 'nexuspass', groupId: 'WebApplication', nexusUrl: 'ec2-3-21-93-162.us-east-2.compute.amazonaws.com:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'
            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
             deploy adapters: [tomcat9(credentialsId: 'tom_pass', path: '', url: 'http://18.118.19.138:8080/')], contextPath: 'myapp', war: '**/*.war'
              
              
              
          }
            
        }
            
        }
} 
