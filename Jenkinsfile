pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
		stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
		stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
		stage('OWASP Dependency-Check Vulnerabilities') {
			steps {
				dependencyCheck additionalArguments: ''' 
						-f 'HTML' 
						--nvdApiKey '7ad48849-c21a-49f4-9ddb-85151d39d039'
						--noupdate
						--prettyPrint''', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
				
				ependencyCheckPublisher pattern: 'dependency-check-report.xml'
			}
		}
    }
}