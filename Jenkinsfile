pipeline {
	agent any
	
	parameters {
	string(name: 'tomcat_dev',
	defaultValue: 'C:/servers/apache-tomcat-9.0.56/webapps',
	description: 'Staging Server : 8080')
	string(name: 'tomcat_prod',
	defaultValue: 'C:/servers/apache-tomcat-9.0.56-prod/webapps',
	description: 'Staging Server : 8090')
	}
	
	triggers {
	pollSCM('* * * * *')
	}
	
	stages {
		stage('Build') {
			steps {
			bat "mvn clean package"
			}
			
			post{
				success {
				echo "Now archiving..."
				archiveArtifacts artifacts : '**/target/*.war'
				}
			}
		}
		
		stage('Deployments') {
			parallel {
				stage('Deploy to staging'){
					steps {
					bat "cp **/target/*.war %tomcat_dev%"
					}
				}
				
				stage('Deploy to production'){
					steps {
					bat "cp **/target/*.war %tomcat_prod%"
					}
				}
			}
		}
	}
}
