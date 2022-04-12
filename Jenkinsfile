pipeline {
  agent any
  stages {
    stage ('build'){
      steps {
        echo 'Build project'
        sh './gradlew build'
        archive 'dist/trainSchedule.zip'
      }
    }
    
    stage ('staging deploy') {
      when {
        branch 'master' 
      }
      steps {
        withCredentials([usernamePassword(credentialsId: 'server_credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]){
          sshPublisher(
            continueOnError: false, failOnError: true,
            publishers: [
              sshPublisherDesc(
                configName: "staging",
                verbose: true,
                transfers: [
                  sshTransfer(
                    sourceFiles: "dist/trainSchedule.zip",
                    removePrefix: "dist",
                    remoteDirectory: "/tmp",
                    execCommand: "echo $PASS | sudo -S unzip /tmp/trainSchedule.zip -d /opt/train-schedule && echo $PASS | sudo -S /usr/bin/systemctl start train-schedule"
                  )
                ]
              )
            ]
          )
        }  
      }
    }
    /*
    stage ('production deploy') {
      when {
        branch 'master' 
      }
      steps {
        input 'Continue the deployment to production?'
        milestone(1)
        sshPublisher(
          continueOnError: false, failOnError: true,
          publishers: [
            sshPublisherDesc(
              configName: "staging",
              verbose: true,
              transfers: [
                sshTransfer(
                  sourceFiles: "/dist/trainSchedule.zip",
                  removePrefix: "/dist",
                  remoteDirectory: "/tmp",
                  execCommand: "sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule"
                )
              ]
            )
          ]
        )
      }
    }
    */
  }
}
