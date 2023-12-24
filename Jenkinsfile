node {
		stage ('checkout code'){
				checkout scm
		}
		
		stage ('Build'){
				sh "mvn clean install -Dmaven.test.skip=true"
		}
		
		stage ('Test Cases Execution'){
				sh "maven clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
		}
		
		stage ('sonar Analaysi'){
				//sh 'mvn sonar:sonar -Dsonar.host.url=http://3.129.195.129:9000 -Dsonar.login=xxxxx'
		}
		
		stage ('Archive	Artifacts'){
				archiveArtifacts artifacts: 'target/*.war'
		}
		
		stage ('Deployment'){
				//deploy adapters: [tomcat9(credentialsId: 'TomcatCred', path: '', url:http://3.129.195.129:8080/')], contextPath: null, war: 'target/*.war'
		}
		
		stage ('Notification'){
				emailext (
						subject: "Job Completed",
						body: "Jenkins pipeline for Job Maven build is completed",
						to:"build-alerts@example.com"
					)
		}
	}
