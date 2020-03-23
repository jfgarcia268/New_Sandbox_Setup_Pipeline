#!/usr/bin/env groovy
pipeline {
node {
  stages {
    stage('Install Tools') {
      steps {
        sh 'node -v'
        sh 'npm install -g vlocity'
        sh 'npm install -g sfdx-cli'
      }
    }
    stage('Setup') {
      steps {
        sh 'vlocity'
      }
    }
    stage('Deploy') {
      steps {
        sh 'sfdx force'
      }
    }
  }
}
}