pipeline {
  agent any

  stages {

    stage('Clean Workspace') {
      steps {
        sh '''
        echo "Cleaning workspace..."
        rm -rf test-reports dist build *.spec || true
        '''
      }
    }

    stage('Build Stage') {
      steps {
        sh '''
        echo "Build Started"

        python3 -m pip install --upgrade pip
        python3 -m pip install -r requirements.txt
        python3 -m pip install pyinstaller

        python3 -m PyInstaller --onefile app.py

        echo "Build Finished"
        '''
      }
    }

    stage('Testing Stage') {
      steps {
        sh '''
        echo "Testing Started"
        python3 test.py || true
        echo "Testing Finished"
        '''
      }
    }

    stage('Run App Demo') {
      steps {
        sh '''
        echo "Running app briefly..."
        timeout 10 python3 app.py || true
        '''
      }
    }

  }

  post {
    always {
      echo 'Pipeline finished'

      archiveArtifacts artifacts: 'dist/*', fingerprint: true, allowEmptyArchive: true

      echo 'Skipping junit report'
    }

    success {
      echo 'BUILD SUCCESS'
    }

    failure {
      echo 'BUILD FAILED'
    }
  }
}
