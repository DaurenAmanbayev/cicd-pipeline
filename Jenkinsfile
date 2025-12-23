pipeline {
  agent any
  stages {
    stage('Git Checkout') {
      steps {
        sh '''    echo "Repository checked out successfully"
    echo "Current branch: $(git branch --show-current || echo \'detached HEAD\')"
    echo "Commit: $(git rev-parse --short HEAD)"
    echo ""
    echo "Project structure:"
    ls -la'''
      }
    }

    stage('Application Build') {
      steps {
        sh './scripts/build.sh'
      }
    }

    stage('Tests') {
      steps {
        sh './scripts/test.sh'
      }
    }

    stage('Docker Image Build') {
      steps {
        sh 'docker build -t discanedocker/cicd-pipeline:latest .'
      }
    }

  }
}