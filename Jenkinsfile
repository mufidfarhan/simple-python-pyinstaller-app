pipeline {
  agent {
    docker {
      image 'python:3.10'
    }
  }
  stages {
    stage('Build') {
      steps {
        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        stash(name: 'compiled-results', includes: 'sources/*.py*')
      }
    }
    stage('Test') {
      steps {
        sh '''
          python3 -m venv venv
          . venv/bin/activate
          pip install pytest
          pytest --junit-xml test-reports/results.xml sources/test_calc.py
        '''
      }
      post {
        always {
          junit 'test-reports/results.xml'
        }
      }
    }
  }
}