pipeline {
    agent { docker { image 'maven:3.3.3' } }
    environment {
      DISABLE_AUTH='true'
      DB_ENGINE='mysql'
    }
    stages {
        stage('build') {
            steps {
                sh 'mvn --version'
                sh 'echo "Hello World"'
                sh '''
                  echo "Database engine is ${DB_ENGINE}"
                  echo "AUTH is ${DISABLE_AUTH}"
                  ls -lah
                '''
            }
        }
        stage('deploy') {
          steps {
            retry(3) {
              sh './scripts/deploy.sh'
            }
            timeout(time: 1, unit: 'MINUTES') {
              sh './scripts/health-check.sh'
            }
          }
        }
        stage('sanit check') {
          steps {
            input 'Everything is ok?'
          }
        }
        stage('test') {
          steps {
            sh 'echo "Fail!";exit 1'
          }
        }
    }
    post {
      always {
          echo 'This will always run'
      }
      success {
          echo 'This will run only if successful'
      }
      failure {
          echo 'This will run only if failed'
      }
      unstable {
          echo 'This will run only if the run was marked as unstable'
      }
      changed {
          echo 'This will run only if the state of the Pipeline has changed'
          echo 'For example, if the Pipeline was previously failing but is now successful'
      }
    }
}
