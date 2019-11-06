pipeline{
    agent any
    
    stages {
        stage ('Init') {
            steps {
                echo "Testing..."
            }
        }
        stage ('Build') {
            steps {
                bat 'mvn clean package'
            }
	    post {
		success {
		    echo "Now Archiving..."
		    archiveArtifacts artifacts: '**/target/*.war'
	}
	}
        }
        stage ('Deploy to staging') {
            steps {
                build job: 'deploy_to_staging'
            }
        }
		stage ('Deploy to Production') {
			steps {
				timeout (time:5, unit: 'DAYS') {
					inpout message: 'Approve PRODUCTION Deployement?'
				}
				build job: 'deploy_to_prod'
			}
			post {
				success {
					echo 'Code deployed to Production.'
				}
				failure {
					echo 'Deployement failed.'
				}
				}
				}
    }
}