pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'bat gradle buid'
        sh 'bat gradle javadoc'
        sh 'bat gradle uploadArchives'
      }
    }
  }
}