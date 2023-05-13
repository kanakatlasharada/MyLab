pipeline{
    //Directives
    agent any
    tools {
        maven 'maven 3.9.1'
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
       //Stage3 : upload artifacts to nexus
        stage ('upload to nexus'){
            steps{
              nexusArtifactUploader artifacts: [[artifactId: 'VinayDevOpsLab', classifier: '', file: 'target/VinayDevOpsLab-0.0.3', type: 'war']], credentialsId: 'cdc0e6c3-e0ff-4526-a1e9-a7dd6f95358e', groupId: 'com.vinaysdevopslab', nexusUrl: '172.31.28.193:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'sharada-firstnexus-repo-SNAPSHOT', version: '0.0.3'  
            }
        }
        // Stage3 : deploying
        stage ('for deploy'){
            steps {
                echo ' Deploying......'
                }

            }
     
    }

}
