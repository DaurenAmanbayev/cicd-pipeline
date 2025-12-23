pipeline {
  agent any
  
  tools {
        nodejs 'node25' 
  }
  
  
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
        sh 'node -v'
        sh 'npm -v'
        sh 'chmod +x scripts/build.sh'
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

    stage('Docker Image Push') {
      steps {
        script {
          docker.withRegistry('') {
            def img = docker.build("discanedocker/cicd-pipeline:latest")
            img.push()
          }
        }

      }
    }

  }
}