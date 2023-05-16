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

        //Stage3 : upload artifacts to nexus
       stage ('Publish to Nexus'){
            steps {
                script {

                def NexusRepo = Version.endsWith("SNAPSHOT") ? "sharada-firstnexus-repo-SNAPSHOT" : "sharada-firstrepo-RELEASE"

                nexusArtifactUploader artifacts: 
                [[artifactId: "${ArtifactId}", 
                classifier: '', 
                file: "target/${ArtifactId}-${Version}.war", 
                type: 'war']], 
                credentialsId: 'cdc0e6c3-e0ff-4526-a1e9-a7dd6f95358e', 
                groupId: "${GroupId}", 
                nexusUrl: '172.31.28.193:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: "${NexusRepo}", 
                version: "${Version}"
             }
            }
        }
        // Stage4 : printing retrieving values
        stage ('printing') {
            steps {
                echo "ArtifactId is '${ArtifactId}'"
                echo "Version is '${Version}'"
                echo "Name is '${Name}'"
                echo "GroupId is '${GroupId}'"
            }
        }
        // Stage5 : deploying
        stage ('for deploy'){
            steps {
                echo ' Deploying......'
                sshPublisher(publishers: 
                [sshPublisherDesc(
                  configName: 'ansible-controller',
                  transfers:
                    [sshTransfer(
                        cleanRemote: false, 
        
                         execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy.yml -i /opt/playbooks/hosts', 
                         execTimeout: 120000,
                         
                                )],
                                 usePromotionTimestamp: false,
                                  useWorkspaceInPromotion: false,
                                   verbose: false
                                   )
                                   ])
                }

            }
     
    }

}

