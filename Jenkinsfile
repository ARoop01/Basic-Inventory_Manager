pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Setup Python Environment') {
      steps {
        sh '''
          python3 --version
        '''
      }
    }

    stage('Install Dependencies') {
      steps {
        sh '''
          pip install -r requirements.txt --break-system-packages
        '''
      }
    }

    stage('Syntax Check') {
      steps {
        sh '''
          python3 -m py_compile app.py
          echo "✓ Python syntax is valid"
        '''
      }
    }

    stage('Validate Data File') {
      steps {
        sh '''
          python3 -c "import json; json.load(open('inventory.json'))"
          echo "✓ inventory.json is valid JSON"
        '''
      }
    }

    stage('Import Test') {
      steps {
        sh '''
          python3 -c "import app; print('✓ Flask app imports successfully')"
        '''
      }
    }

    stage('Test Summary') {
      steps {
        echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
        echo "✓ All checks passed!"
        echo "  - Python syntax valid"
        echo "  - Dependencies installed"
        echo "  - Data file valid"
        echo "  - App imports successfully"
        echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
      }
    }
  }

  post {
    success {
      echo "✓ Pipeline completed successfully"
    }
    failure {
      echo "✗ Pipeline failed - check logs above"
    }
  }
}
