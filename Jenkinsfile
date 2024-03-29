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
		stage('Deploy to Test Environment') {
			steps {
				echo 'Celonis web app was deployed to Test environment'			
			}
		}
		stage('Execute Smoke Tests') {
		    when {
		        tag "hotFix-*"
		    }
			steps {
				git branch: 'master',
    				credentialsId: '76a3d309-924e-4a51-ae24-de38b36806e8',
    				url: 'https://github.com/sergeyfilin/cloud.celonis.testautomation.app.git'
    		    sh "./gradlew clean test -Dtags=smoke"
    		}
    		post {
				always {
					publishHTML target: [
						allowMissing: false,
						alwaysLinkToLastBuild: false,
						keepAll: true,
						reportDir: 'target/site/serenity/',
						reportFiles: 'index.html',
						reportName: 'Smoke Test Report'
					]
				}	
			}	
		}
		stage('Execute Regression Tests') {
		    steps {
				git branch: 'master',
    				credentialsId: '76a3d309-924e-4a51-ae24-de38b36806e8',
    				url: 'https://github.com/sergeyfilin/cloud.celonis.testautomation.app.git'
    		    sh "./gradlew clean test"
    		}
    		post {
				always {
					publishHTML target: [
						allowMissing: false,
						alwaysLinkToLastBuild: false,
						keepAll: true,
						reportDir: 'target/site/serenity/',
						reportFiles: 'index.html',
						reportName: 'Regression Test Report'
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