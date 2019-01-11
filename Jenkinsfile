pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat 'C:\\Users\\dell\\Documents\\gradle-4.10.2\\bin\\gradle build'
        bat 'C:\\Users\\dell\\Documents\\gradle-4.10.2\\bin\\gradle javadoc'
        bat 'C:\\Users\\dell\\Documents\\gradle-4.10.2\\bin\\gradle uploadArchives'
      }
    }
  }
}