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
         when (
             branch 'master'
             }
             steps {
              withCredentials ([usernamePassword(credentialsId: 'crimson_pwd' , usernameVariable: 'USERNAME' , passwordVarible: 'PASSWD"
                       sshPublisher(
                          failOnError: true,
                          continueOnError: false,
                          publishers: [
                              sshPublisherDesc(
                                  configName: 'stag'
                                  sshCredentials: [
                                      username: "$USERNAME",
                                      encyptedPasspharse: "$PASSWD"
                                      ],
                                  transfers: [
                                      sshTransfer(
                                          sourcefiles: 'dist/tain.zip',
                                          removePrefix: 'dist/',
                                          remoteDirctory: '/temp',
                                          
             }
        }
    }
}
