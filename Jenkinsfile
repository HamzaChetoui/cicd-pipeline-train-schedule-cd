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
                  execCommand: "pwd"
                )
              ]
            )
          ]
        )
      }
    }
    
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
                  execCommand: "pwd"
                )
              ]
            )
          ]
        )
      }
    }
  }
}
