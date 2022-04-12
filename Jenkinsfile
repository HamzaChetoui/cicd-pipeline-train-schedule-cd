pipeline {
  agent any
  stages {
    stage ('build'){
      steps {
        echo 'Build project'
        sh 'gradlew build'
        archive 'dist/trainSchedule.zip'
      }
    }
  }
}
