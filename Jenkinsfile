pipeline {
	agent any
	tools {
		nodejs 'NodeJS'
	}
	environment {
		SONAR_PROJECT_KEY = 'complete-cicd-02'
		SONAR_SCANNER_HOME = tool 'SonarQubeScanner'
	}
	stages {
		stage('GitHub'){
			steps {
				git branch: 'main', credentialsId: 'jen-git-dind', url: 'https://github.com/iQuantC/Complete_CICD_02.git'
			}
		}
		stage('Unit Test'){
			steps {
				sh 'npm test'
				sh 'npm install'		
			}
		}
		stage('SonarQube Analysis'){
			steps {
				withCredentials([string(credentialsId: 'complete-cicd-02-token', variable: 'SONAR_TOKEN')]) {
    					
					withSonarQubeEnv('SonarQube') {
    						sh """
						${SONAR_SCANNER_HOME}/bin/sonar-scanner \
						-Dsonar.projectKey=${SONAR_PROJECT_KEY} \
						-Dsonar.sources=. \
						-Dsonar.host.url=http://sonarqube-dind:9000 \
						-Dsonar.login=${SONAR_TOKEN}
						"""
					}
				}
			}
		}
		stage('Docker Image'){
			steps {
				script {
					docker.build("${BUILD_ID}:latest")
				}
			}
		}
		stage('Trivy Scan'){
			steps {
				sh 'trivy --severity HIGH,CRITICAL --no-progress --format table -o trivy-report.html image ${BUILD_ID}:latest'
			}
		}
	}
}