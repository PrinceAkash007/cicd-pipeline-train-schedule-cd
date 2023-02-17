pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('DeployToStaging') {
            when {
                branch 'master'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'crimson_pwd', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'stag',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/trainSchedule.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
       stage('DeployToStaging') {
           when {
               branch 'master'
           }
           steps {
               input 'is the stagging working properly'
               milestone(1)
               withCredentials([usernamePassword(credentialsId: 'crimson_pwd', usernameVariable: 'id', passwordVariable: 'password')]){
                sshPublisher(
                    failOnError: true,
                    continueOnError: false,
                    publishers: [
                        sshPublisherDesc(
                        configName: 'prod',
                            ssCredentials: [
                                username: "$id",
                                encryptedPassphrase: "$password"
                                ],
                            transfers: [
                                sshTransfer(
                                    sourceFile: 'dist/trainSchedule.zip',
                                    removePrefix: 'dist/',
                                    romoteDirectory: '/temp'
                                    execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /temp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                                    
                                    )
                                ]
                            )
                        ]
                    )
           }
       }
            
    }
 }

