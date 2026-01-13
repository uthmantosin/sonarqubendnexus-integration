pipeline {
    agent any
    environment {
      MAVEN_OPTS="--add-opens java.base/java.lang=ALL-UNNAMED"   
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
                  withSonarQubeEnv('sonar_scanner') {
                                     

             sh "mvn -f SampleWebApp/pom.xml sonar:sonar"      
               }
            }
       }
        stage('push to nexus') {
            steps {
                    nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'war']], credentialsId: 'nexuspasswd', groupId: 'SampleWebApp', nexusUrl: 'ec2-44-203-170-122.compute-1.amazonaws.com:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.1-SNAPSHOT'
            }  
            
        }
        
        stage('deploy to tomcat') {
          steps {
              deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'tomcatpasswd', path: '', url: 'http://44.211.200.86:8080/')], contextPath: 'mono', war: '**/*.war'
          }
            
        }
            
        }
} 
