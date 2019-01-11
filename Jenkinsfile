pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'gradle buid'
        sh 'gradle javadoc'
        sh 'gradle uploadArchives'
      }
    }
  }
}