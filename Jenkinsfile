pipeline {
  agent none
  stages {
    stage('Run Pytest Suites in Parallel') {
      parallel {
        stage('Sanity Suite') {
          agent {
            kubernetes {
              yamlFile 'pod-sanity.yaml'
            }
          }
          steps {
            sh 'pytest -m sanity -n 2 --html=reports/report-sanity.html'
          }
          post {
            always {
              archiveArtifacts artifacts: 'reports/report-sanity.html', allowEmptyArchive: true
            }
          }
        }
        stage('Regression Suite') {
          agent {
            kubernetes {
              yamlFile 'pod-regression.yaml'
            }
          }
          steps {
            sh 'pytest -m regression -n 2 --html=reports/report-regression.html'
          }
          post {
            always {
              archiveArtifacts artifacts: 'reports/report-regression.html', allowEmptyArchive: true
            }
          }
        }
        stage('Parametrize Suite') {
          agent {
            kubernetes {
              yamlFile 'pod-parametrize.yaml'
            }
          }
          steps {
            sh 'pytest -m parametrize -n 2 --html=reports/report-parametrize.html'
          }
          post {
            always {
              archiveArtifacts artifacts: 'reports/report-parametrize.html', allowEmptyArchive: true
            }
          }
        }
      }
    }

    stage('Send Email') {
      steps {
        emailext (
          subject: "Pytest Report",
          body: "Test execution completed. Please find the attached HTML reports.",
          to: "deepthi1987.p@gmail.com",
          attachLog: false,
          attachmentsPattern: "reports/*.html"
        )
      }
    }
  }
}
