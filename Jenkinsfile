pipeline {
    agent any
    // agent {
    //     docker {
    //         image 'maven:3-alpine' 
    //         args '-v /root/.m2:/root/.m2' 
    //     }
    // }
    stages {
       // agent any
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }

    stage('Sonarqube') {
      //  agent any
        environment {
            scannerHome = tool 'SonarQubeScanner'
        }
        steps {
            withSonarQubeEnv('sonarqube') {
                sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=myproject -Dsonar.sources=./src/main/java/ -Dsonar.java.binaries=./target/classes -Dsonar.language=java"
            }
            timeout(time: 10, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
            }
        }
    }

		// stage('Test') {
        //     steps {
        //         sh 'mvn test'
        //     }
        //     post {
        //         always {
        //             junit 'target/surefire-reports/*.xml'
        //         }
        //     }
        // }
		// stage('Deliver to develop') {
		//     when {
        //         branch 'develop'
        //     }
        //     steps {
        //         sh './jenkins/scripts/deliver.sh' 
        //     }
        // }
    }
}