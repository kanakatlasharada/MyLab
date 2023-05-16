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
        stage ('upload to nexus'){
            steps{
               nexusArtifactUploader artifacts: [[artifactId: 'VinayDevOpsLab', classifier: '', file: 'target/VinayDevOpsLab-0.0.1-SNAPSHOT.war', type: 'war']], credentialsId: 'fc3b5076-171d-4bfe-9ecd-ed9cafa48edd', groupId: 'com.vinaysdevopslab', nexusUrl: '35.174.170.220:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'sharadarepo-SNAPSHOT', version: '0.0.1-SNAPSHOT'
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
                    transfers: [
                        sshTransfer(
                            cleanRemote: false, 
                            excludes: '', 
                            execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy.yml -i /opt/playbooks/hosts', 
                            execTimeout: 120000, 
                            flatten: false, 
                            makeEmptyDirs: false, 
                            noDefaultExcludes: false, 
                            patternSeparator: '[, ]+', 
                            remoteDirectory: '/opt/apache-tomcat-9.0.75/webapps',
                            remoteDirectorySDF: false,
                            removePrefix: '/home/ansadmin', 
                            sourceFiles: '/home/ansadmin/*.war')], 
                            usePromotionTimestamp: false, 
                            useWorkspaceInPromotion: false, 
                            verbose: false)])
                }

            }
     
    }

}

