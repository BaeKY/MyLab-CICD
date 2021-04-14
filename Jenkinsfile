pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    environment {
        ArtifactId = readMavenPom().getArtifactId()
        Version    = readMavenPom().getVersion()
        GroupId    = readMavenPom().getGroupId()
        Name       = readMavenPom().getName()
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
        stage ('publish to Nexus') {
            steps {
                script {
                    def NexusRepo = Version.endsWith("SNAPSHOT") ? "baeKYDevOpsLab-SNAPSHOT" : "BaeKYDevOpsLab-RELEASE"
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: "${ArtifactId}", 
                            classifier: '', 
                            file: 'target/BaeKYDevOpsLab-0.0.4-SNAPSHOT.war', 
                            type: 'war'
                        ]
                    ], 
                    credentialsId: '4db5b5a7-7aa7-465c-84d0-79016e99478c', 
                    groupId: "${GroupId}", 
                    nexusUrl: '172.20.10.214:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: "${NexusRepo}", 
                    version: "${Version}"
                }
            }
        }

        // Stage 4 : Print some informations
        stage ('Print Environment variables') {
            steps {
                echo "ArtifactID is '${ArtifactId}'"
                echo "Version is '${Version}'"
                echo "GroupID is '${GroupId}'"
                echo "Name is '${Name}'"
            }
        }

        // Stage 5 : Deploying
        stage ('Deploy'){
            steps {
                echo ' deploying..........................'

            }
        }

    }

}