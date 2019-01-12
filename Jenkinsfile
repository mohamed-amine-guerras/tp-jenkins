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
    stage('Mail Notification') {
      steps {
        mail(subject: 'Build Succeded!', body: 'the last build has succeded', from: 'jenkins-notification@jenkins.com', to: 'fm_guerras@esi.dz')
      }
    }
    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          environment {
            scannerHome = tool 'sonar-scanner'
          }
          steps {
            withSonarQubeEnv('sonarqube') {
              bat "${scannerHome}\\sonar-scanner"
            }

            timeout(time: 10, unit: 'MINUTES') {
              waitForQualityGate true
            }

          }
        }
        stage('Test Reporting') {
          steps {
            jacoco()
          }
        }
      }
    }
    stage('Deployment') {
      steps {
        bat 'C:\\Users\\dell\\Documents\\gradle-4.10.2\\bin\\gradle uploadArchives'
      }
    }
    stage('Slack Notification') {
      steps {
        slackSend(color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
      }
    }
  }
}
