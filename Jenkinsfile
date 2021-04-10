pipeline {
  agent any
  stages {
      stage('Pre-Build') {
        steps {
          echo 'Pre Building...'
          echo "Running ${env.BUILD_ID} ${env.BUILD_DISPLAY_NAME} on ${env.NODE_NAME} and JOB ${env.JOB_NAME}"
          sh 'pip install --upgrade pip'
          sh 'pip install -r requirements.txt'
          sh 'rm -rf dspt'
          sh 'mkdir test-reports'
        }
   }
   stages {
      stage('Build') {
        steps {
          echo 'Building...'
        }
   }
   stage('Test') {
     steps {
        echo 'Testing...'
        sh 'flake8 --format junit-xml --output-file test-reports/flake8_report.xml || echo "Check Linting Errors"
        sh "pytest -s -v --cov --cov-report=html:test-reports/coverage --junitxml=test-reports/coverage/pytest_report.xml --log-file=test-reports/logs.txt "
     }
   }
   stage('Deploy') {
     steps {
       echo 'Deploying...'
       sh "ls -lR ./test-reports"
     }
   }
  }
}
