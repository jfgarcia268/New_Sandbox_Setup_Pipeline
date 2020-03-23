#!/usr/bin/env groovy
pipeline {
  agent any
  tools {nodejs "PipelineNode"}
  stages {
    stage('Install Tools') {
      steps {
      	// Step to Install and Setup VBT and SFDX-CLI
        sh 'node -v'
        sh 'npm install -g vlocity sfdx-cli'
        sh 'npm install -g sfdx-cli'
      }
    }
    stage('SFDX-Auth') {
      steps {
      	// creating SFDX Alias for auth
        sh 'echo $SFDX_URL'
		sh 'echo ${SFDX_URL} > env.sfdx'
		sh 'sfdx force:auth:sfdxurl:store -d -a ${SFDX_URL} -f env.sfdx'
		sh 'rm -rf env.sfdx'
		sh 'sfdx force:org:display -u ${SFDX_URL}'
      }
    }
    stage('SF Deploy') {
      steps {
      	// SF Metadata Deploy
		sh 'sfdx force:source:deploy --sourcepath salesforce_sfdx --targetusername ${SFDX_URL} --verbose'
		// Data Deployment (Optional)
		sh 'sfdx force:data:tree:import --targetusername ${SFDX_URL} --plan sfdx-data/Account-plan.json || true'
		// Permission set assignment (Optional)
		sh 'sfdx force:user:permset:assign --targetusername ${SFDX_URL} --permsetname HandsetBuy'
      }
    }
    stage('Vlocity 1st Time Setup') {
      steps {
      	// Vlocity 1st time Setup commands 
      	// Env VBT Update 
		sh 'vlocity -sfdx.username "$ALIAS" --nojob packUpdateSettings --verbose true --simpleLogging true'
		// Install OOTB Vlocity DataPacks
		sh 'vlocity -sfdx.username "$ALIAS" --nojob installVlocityInitial --verbose true --simpleLogging true'
		// Apex 1st time setup (Optional)
		sh 'vlocity -sfdx.username "$ALIAS" --nojob runApex -apex apex/cmt_InitializeOrg.cls --verbose true --simpleLogging true'
      }
    }
    stage('Vlocity Deploy') {
      steps {
      	// VBT Deploy
      	sh 'vlocity -sfdx.username "$ALIAS" -job Deploy_Delta.yaml packDeploy --verbose true --simpleLogging true'
        // Apex Post Deplyment Jobs (Optional)
        sh 'vlocity -sfdx.username "$ALIAS" --nojob runApex -apex apex/RunProductBatchJobs.cls --verbose true --simpleLogging true'
      }
    }
  }  
}