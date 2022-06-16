pipeline {
   tools {
        jdk 'java9'
    }
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
       
      steps {
                withCredentials([usernameKey(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERKEY')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'staging',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    key: "$USERKEY"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/trainSchedule.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'echo done'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
