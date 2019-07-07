pipeline {
    agent none
    stages {
        stage('Build') { 
            agent {
                docker {
                    image 'maven:3-alpine' 
                    args '-v /root/.m2:/root/.m2' 
                }
            }
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }

    stage('Sonarqube') {
        agent {
            docker {
                image 'sonar-scanner:1.0.0' 
                args '-v /root/.m2:/root/.m2' 
            }
        }
        steps {
            withSonarQubeEnv('sonarqube') {
                sh "sonar-scanner -Dsonar.projectKey=myproject -Dsonar.sources=./src/main/java/ -Dsonar.java.binaries=./target/classes -Dsonar.language=java"
            }
            timeout(time: 10, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
            }
        }
    }

		stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
		stage('Deliver to develop') {
		    when {
                branch 'develop'
            }
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
    }
}