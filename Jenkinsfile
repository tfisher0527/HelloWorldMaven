pipeline { 
    agent any 
    stages {
        stage('Build') { 
            steps {
                withMaven(maven : 'apache-maven-3.9.5'){
                        sh "mvn clean compile"
                }
            }
        }
        stage('Test'){
            steps {
                withMaven(maven : 'apache-maven-3.9.5'){
                        sh "mvn test"
                }

            }
        }
	stage('build && SonarQube analysis') {
	    steps {
		    script{
	            withSonarQubeEnv('sonar-scanner') {
			def scannerHome = tool name: 'sonar-scanner', type: 'SonarQube Scanner'
	                sh '${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=myProject -Dsonar.sources=./src'
		    }
	        }
	    }
	}
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    // Requires SonarScanner for Jenkins 2.7+
                    waitForQualityGate abortPipeline: true
                }
            }
			}
        stage('Deploy') {
            steps {
               withMaven(maven : 'apache-maven-3.9.5'){
                        sh "mvn deploy"
                }

            }
        }
    }
}
