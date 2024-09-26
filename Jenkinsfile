pipeline {
	agent any
	tools {
		nodejs 'NodeJS'
	}
	environment {
		SONAR_PROJECT_KEY = 'do12'
		SONAR_SCANNER_HOME = tool 'sonar-scanner'
	}

	stages {
		stage('Checkout Github'){
			steps {
				git branch: 'main', credentialsId: 'git-token', url: 'https://github.com/rishi-patil/Todo.git'
			}
		}

		stage('Install node dependencies'){
			steps {
				// Use 'bat' for Windows instead of 'sh'
				bat 'npm install'
			}
		}
		stage('Tests'){
			steps {
				// Use 'bat' for running tests on Windows
				bat 'npm test'
			}
		}
		stage('SonarQube Analysis'){
			steps {
				withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
					withSonarQubeEnv('sonarqube') {
						// Replace shell (sh) commands with Windows batch script (bat) compatible syntax
						bat """
                  				${SONAR_SCANNER_HOME}\\bin\\sonar-scanner.bat ^
                  				-Dsonar.projectKey=${SONAR_PROJECT_KEY} ^
                   				-Dsonar.sources=. ^
                   				-Dsonar.host.url=http://localhost:9000 ^
                   				-Dsonar.login=%SONAR_TOKEN%
                    		"""
					}	
				}
			}
		}
	}
	post {
		success {
			echo 'Build completed successfully!'
		}
		failure {
			echo 'Build failed. Check logs.'
		}
	}
}
