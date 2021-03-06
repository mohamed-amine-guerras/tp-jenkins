pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat 'C:\\Users\\dell\\Documents\\gradle-4.10.2\\bin\\gradle build'
        bat 'C:\\Users\\dell\\Documents\\gradle-4.10.2\\bin\\gradle javadoc'
        archiveArtifacts 'build/libs/*jar'

      }
     post {
        success {
          mail(subject: 'SUCCESSFUL Job', body: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})", from: 'jenkins-notification@jenkins.com', to: 'fm_guerras@esi.dz')
        }
        failure {
          mail(subject: "FAILED Job", body: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})", from: 'jenkins-notification@jenkins.com', to: 'fm_guerras@esi.dz')
        }
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
              waitForQualityGate abortPipeline: true
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
      when {
        branch 'master'
      }
      steps {
        bat 'C:\\Users\\dell\\Documents\\gradle-4.10.2\\bin\\gradle uploadArchives'
      }
    }
    stage('Slack Notification') {
      when {
        branch 'master'
      }
      steps {
        slackSend(color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
      }
    }
  }
   post {
        always {
            allure results: [[path: 'build/test-results/test']]
        }
   }
}
