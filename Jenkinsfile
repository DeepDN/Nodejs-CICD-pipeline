pipeline {
	agent any
	tools{
		nodejs 'NodeJS'
}
	stages{
		stage('git code colne'){
			steps{
				git branch: 'main', credentialsId: 'PERSONAL_GIT_TOKEN_CREDENTIALS', url: 'https://github.com/DeepDN/Nodejs-CICD-pipeline.git'
	}
	}
		stage('Unit test'){
                        steps{
                             sh '''npm test
npm install'''    
        }
        }

	}
	}
