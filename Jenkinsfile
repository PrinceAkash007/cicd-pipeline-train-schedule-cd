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
        stage('Deploy'){
            when {
             branch 'master'
             }
             steps {
              withCredentials ([usernamePassword(credentialsId: 'crimson_pwd' , usernameVariable: 'USERNAME' , passwordVarible: 'PASSWD')])
                       sshPublisher(
                          failOnError: true,
                          continueOnError: false,
                          publishers: [
                              sshPublisherDesc(
                                  configName: 'stag',
                                  sshCredentials: [
                                      username: "$USERNAME",
                                      encyptedPasspharse: "$PASSWD"
                                      ],
                                  transfers: [
                                      sshTransfer(
                                          sourcefiles: 'dist/train.zip',
                                          removePrefix: 'dist/',
                                          remoteDirctory: '/temp',
                                         execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                                          )
                                      ]
                                  )
                              ]
                           )
                         }
                                                 
                   }
             }
        }
