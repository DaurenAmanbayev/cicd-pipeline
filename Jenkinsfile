pipeline {
    agent none
    
    environment {
        npm_config_cache = "${env.WORKSPACE}/.npm-cache"
    }
  stages {
    stage('Git Checkout') {
	    agent {
        docker { 
            image 'library/node:20-bullseye-slim'
            args '-u root:root' 
            }
        }
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
		agent {
        docker { 
            image 'library/node:20-bullseye-slim'
            args '-u root:root' 
            }
        }
        steps {
        sh 'node -v'
        sh 'npm -v'
        sh 'chmod +x scripts/build.sh'
        sh './scripts/build.sh'
      }
    }

    stage('Tests') {
	    agent {
        docker { 
            image 'library/node:20-bullseye-slim'
            args '-u root:root' 
            }
        }
        steps {
        sh './scripts/test.sh'
      }
    }

    stage('Docker Image Build') {
	   agent { label 'docker' }
       steps {
        sh 'docker build -t discanedocker/cicd-pipeline:latest .'
      }
    }

    stage('Docker Image Push') {
	   agent { label 'docker' }
       steps {
        script {
          docker.withRegistry('') {
            sh 'docker push discanedocker/cicd-pipeline:latest'
          }
        }

      }
    }

  }
}