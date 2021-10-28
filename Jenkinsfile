pipeline {
  agent any
  environment {
      MAJOR = '1'
      MINOR = '0'
	    //Orchestrator Services
	  UIPATH_ORCH_URL = "https://orch-gourav.tam.local/"
	        //UIPATH_ORCH_LOGICAL_NAME = "anupaminc"
	  UIPATH_ORCH_TENANT_NAME = "Default"
	  UIPATH_ORCH_FOLDER_NAME = "hellowworld"
  }
  stages {
      stage('Build') {
          steps {
             
              UiPathPack (
			  outputPath: "${env.JENKINS_HOME}\\jobs\\${env.JOB_NAME}\\builds\\${env.BUILD_NUMBER}", projectJsonPath: "${env.WORKSPACE}", 
			  version: [$class: 'ManualEntry', text: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
			   traceLevel: "None", 
			  )
			   
          }
      }
      stage('Staging Environment') {
          steps {
              mail bcc: '', body: " Hi, \n \n Please follow the link below to approve deployment into Staging. \n \n http://192.168.2.36:61000/job/UiPath-Deployment-Pipeline/ \n \n Regards, \n Ashraya" , cc: '', from: 'jenkins@uipath.com', replyTo: '', subject: 'Your Approval Required', to: env.APPROVERS
              timeout(time: 14, unit: 'DAYS') {
                  input message: 'Please approve the deployment to staging', submitter:"ashraya,paladinsindia"
              }
              echo "Deploying ${BRANCH_NAME} to orchestrator"
	                UiPathDeploy (
	                packagePath: "C:\jenkinsfolder\\${env.JOB_NAME}\\builds\\${env.BUILD_NUMBER}",
	                orchestratorAddress: "${UIPATH_ORCH_URL}",
	                orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
	                folderName: "${UIPATH_ORCH_FOLDER_NAME}",
	                environments: 'INT',
					 traceLevel: "None",
					 entryPointPaths:"main.xaml",
	                credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'c61a1f5a-8502-4371-b927-b4bfd62d220c']
	                //credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'APIUserKey'), 
	

					)
          }
      }
      stage('Production Environment') {
          steps {
              mail bcc: '', body: " Hi, \n \n Please follow the link below to approve deployment into production. \n \n http://192.168.2.36:61000/job/UiPath-Deployment-Pipeline/ \n \n Regards, \n Ashraya", cc: '', from: 'jenkins@uipath.com', replyTo: '', subject: 'Your Approval Required', to: env.APPROVERS
              timeout(time: 14, unit: 'DAYS') {
                  input message: 'Please approve the deployment to Production', submitter:"ashraya"
              }
              echo "Deploying ${BRANCH_NAME} to orchestrator"
	                UiPathDeploy (
	                packagePath: "C:\jenkinsfolder\\${env.JOB_NAME}\\builds\\${env.BUILD_NUMBER}",
	                orchestratorAddress: "${UIPATH_ORCH_URL}",
	                orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
	                folderName: "${UIPATH_ORCH_FOLDER_NAME}",
	                environments: 'INT',
					 traceLevel: "None",
					 entryPointPaths:"main.xaml",
	                credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'c61a1f5a-8502-4371-b927-b4bfd62d220c']
	                //credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'APIUserKey'), 
	

					)
          }
      }
 }
 post {
      success {
          echo "Process ${env.GIT_URL} with version ${MAJOR}.${MINOR}.${env.BUILD_NUMBER} was successfully deployed into Production."
      }
  }
 }