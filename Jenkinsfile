pipeline {
   agent any
	

	        // Environment Variables
	        environment {
	        MAJOR = '1'
	        MINOR = '1'
	        //Orchestrator Services
	        UIPATH_ORCH_URL = "https://orch-gourav.tam.local/"
	        //UIPATH_ORCH_LOGICAL_NAME = "anupaminc"
	        UIPATH_ORCH_TENANT_NAME = "Default"
	        UIPATH_ORCH_FOLDER_NAME = "hellowworld"
	    }
	

	    stages {
	

	        // Printing Basic Information
	        stage('Preparing'){
	            steps {
	                echo "Jenkins Home ${env.JENKINS_HOME}"
	                echo "Jenkins URL ${env.JENKINS_URL}"
	                echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
	                echo "Jenkins JOB Name ${env.JOB_NAME}"
	                echo "GitHub BranchName ${env.BRANCH_NAME}"
	                checkout scm
	

	            }
	        }
	

	         // Building Artifact
	        stage('Packaging') {
	            steps {
	                echo "Building package with ${WORKSPACE}"
	                UiPathPack (
	                      outputPath: "Output\\Tests\${env.BUILD_NUMBER}",
						  outputType: 'Tests',
	                      projectJsonPath: "project.json",
	                      version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
	                      useOrchestrator: false,
						  traceLevel: "None",
						)
	            }
	        }
			
	         // Deploy Stages
	        stage('Deploy Environment') {
	            steps {
	                echo "Deploying ${BRANCH_NAME} to orchestrator"
	                UiPathDeploy (
	                packagePath: "Output\\Tests\${env.BUILD_NUMBER}",
	                orchestratorAddress: "${UIPATH_ORCH_URL}",
	                orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
	                folderName: "${UIPATH_ORCH_FOLDER_NAME}",
	                environments: 'INT',
					traceLevel: "None",
					entryPointPaths:"main.xaml"
	                credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'c61a1f5a-8502-4371-b927-b4bfd62d220c']
	                //credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'APIUserKey'), 
	

					)
	            }
			
			}
}
}
