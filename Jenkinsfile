pipeline{
    //Directives
    agent any
    tools {
        maven 'maven 3.9.1'
    }
    environment{
        ArtifactId = readMavenPom().getArtifactId() 
        Version = readMavenPom().getVersion()
        Name =  readMavenPom().getName()
        GroupId = readMavenPom().getGroupId()
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }
        stage('SonarQube analysis') {
            steps{
                echo "source code published sonarqube for SCA..."
                 withSonarQubeEnv('sonarqube') { // You can override the credential to be used
                 sh 'mvn sonar:sonar'
       }
     } 
   }
 }
} 