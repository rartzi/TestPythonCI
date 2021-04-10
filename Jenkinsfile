pipeline {
  agent any
  stages {
    stage('Pre-Build') {
        steps {
          echo 'Pre Building...'
          echo "Running ${env.BUILD_ID} ${env.BUILD_DISPLAY_NAME} on ${env.NODE_NAME} and JOB ${env.JOB_NAME}"
          sh 'python3 -m pip install --upgrade pip'
          sh 'python3 -m pip install -r requirements.txt'
          sh 'rm -rf dspt'
          sh 'mkdir -p test-reports'
        }
    }
    stage('Build') {
          steps {
            echo 'Building...'
            sh 'python3 -m pip install twine'
            sh 'python3 setup.py sdist bdist_wheel'
            sh 'aws codeartifact login --tool twine --domain astrazeneca-preprod --repository my-repository --region eu-west-1'
            sh 'python3 -m twine upload dist/* --repository codeartifact'
          }
    }
    stage('Test-Linting') {
      steps {
          echo 'Linting...'
          sh 'python3 -m flake8 --format junit-xml --output-file test-reports/flake8_report.xml '
      }
    }
    stage('Test-UnitTestsCoverage') {
      steps {
          echo 'Testing...'
          sh "python3 -m pytest -s -v --cov --cov-report=html:test-reports/coverage --junitxml=test-reports/coverage/pytest_report.xml --log-file=test-reports/logs.txt "
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