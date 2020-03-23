#!/usr/bin/env groovy
pipeline {
  agent any
  tools {nodejs "PipelineNode"}
  stages {
    stage('Install Tools') {
      steps {
        sh 'node -v'
        sh 'npm install -g vlocity sfdx-cli'
        sh 'npm install -g sfdx-cli'
      }
    }
    stage('SFDX-Auth') {
      steps {
        sh 'echo $SFDX_URL'
		sh 'echo ${SFDX_URL} > env.sfdx'
		sh 'ALIAS=${SFDX_URL}'
		sh 'ls -la'
		sh 'cat env.sfdx'
		sh 'sfdx force:auth:sfdxurl:store -d -a ${ALIAS} -f env.sfdx'
		sh 'rm -rf env.sfdx'
		sh 'sfdx force:org:display -u $ALIAS'
      }
    }
    stage('Deploy') {
      steps {
        sh 'sfdx force:org:display -u $ALIAS'
        sh 'echo $ALIAS'
      }
    }
  }
}