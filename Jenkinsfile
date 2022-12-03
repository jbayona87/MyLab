pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    environment {
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getVersion()
        Name = readMavenPom().getName()
        GrupoId = readMavenPom().getGroupId()
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

        // Stage3 : Publish the artifacts to Nexus
        // stage ("Publish to Nexys") {
        //     steps {
        //         script {
        //             def NexusRepo = Version.endsWith("SNAPSHOT") ? "VinayDevOpsLab-SNAPSHOT" : "VinayDevOpsLab-RELEASE"
        //             nexusArtifactUploader artifacts: 
        //             [[artifactId: "${ArtifactId}", 
        //             classifier: '', 
        //             file: "target/${ArtifactId}-${Version}.war", 
        //             type: 'war']], 
        //             credentialsId: '78304c51-8f25-4d07-b5bc-58470ecaec04', 
        //             groupId: "${GrupoId}", 
        //             nexusUrl: '172.20.10.196:8081', 
        //             nexusVersion: 'nexus3', 
        //             protocol: 'http', 
        //             repository: "${NexusRepo}", 
        //             version: "${Version}"
        //         }
        //     }
        // }

        // Stage 4 : Print some information
        stage ("Print Environment variables"){
            steps {
                echo "Artifact ID is '${ArtifactId}'"
                echo "Version is '${Version}'"
                echo "GrupoID is '${GrupoId}'"
                echo "Name is '${Name}'"
            }
        }

        // // Stage3 : Publish the source code to Sonarqube
        // stage ('Sonarqube Analysis'){
        //     steps {
        //         echo ' Source code published to Sonarqube for SCA......'
        //         withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
        //              sh 'mvn sonar:sonar'
        //         }

        //     }
        // }

        // Stage 5 : Deploying Jenkinsfile
        stage ("Deploy"){
            steps {
                echo "Deploying"
                sshPublisher(publishers: 
                [sshPublisherDesc(configName: 'Ansible_Controller', 
                transfers: [
                    sshTransfer(
                        cleanRemote: false, 
                        // excludes: '', 
                        execCommand: 'ansible-playbook /opt/playbooks/archive.yaml', 
                        execTimeout: 120000 //, 
                        // flatten: false, 
                        // makeEmptyDirs: false, 
                        // noDefaultExcludes: false, 
                        // patternSeparator: '[, ]+', 
                        // remoteDirectory: '', 
                        // remoteDirectorySDF: false, 
                        // removePrefix: '', 
                        // sourceFiles: ''
                        )], 
                usePromotionTimestamp: false, 
                useWorkspaceInPromotion: false, 
                verbose: false)])

            }
        }

        
        
    }

}