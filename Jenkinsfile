pipeline {
  agent any
  tools { 
        maven 'Maven_3.2.5'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=asgbuggywebappdanny_usr-share-apache-maven -Dsonar.organization=asgbuggywebappdanny -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=2967b488da91e6e6fa943489e9b0001e58b34176'
			}
        } 
	   stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    	}
	   stage('Build') {
		   steps {
			   withDockerRegistry([credentialsId: "dockelogin", url: ""]) {
				   script {
					   app = docker.build("asg")
				   }
			   }
		   }
	}
	   stage('Push') {
		   steps {
			   script {
				   docker.withRegistry('https://145988340565.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:aws-credentials') {
					   app.push("latest")
				   }
			   }
		   }
	   }
   }
  }
}
