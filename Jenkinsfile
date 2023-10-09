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
		    withSonarQubeEnv('sonar-scanner') {
			withMaven(maven : 'apache-maven-3.9.5'){
			sh 'mvn sonar:sonar -Dsonar.projectKey=myProject -Dsonar.sources=./src/main/java -Dsonar.tests=./src/test/java'
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
               withMaven(maven : 'apache-maven-3.9.5',mavenSettingsConfig: 'c292b638-1ea3-4574-9e21-056b514d554f'){
                        sh "mvn deploy"
                }

            }
        }
    }
}
