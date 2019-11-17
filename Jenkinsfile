pipeline {

	agent any 
	
	options {
		buildDiscarder(logRotator(numToKeepStr: '3'))
		disableConcurrentBuilds()
	}

	stages {
		stage('Build') {
			steps {
				echo 'Celonis web app build was created'		
			}
		}
		stage('Deploy to Test environment') {
			steps {
				echo 'Celonis web app was deployed to Test environment'			
			}
		}
		stage('Execute E2E tests') {
			steps {
				echo 'Celonis web app was under the E2E tests'	
			}
			post {
				always {
					publishHTML target: [
						allowMissing: false,
						alwaysLinkToLastBuild: false,
						keepAll: true,
						reportDir: 'target/site/serenity/',
						reportFiles: 'index.html',
						reportName: 'Test Report'
					]
				}	
			}	
		}
	}

	post {
		always {
			cleanWs()
			dir("${env.WORKSPACE}@tmp") {deleteDir()}
		}
	}
}