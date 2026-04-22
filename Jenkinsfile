pipeline {
    agent any

    tools {
        // These names must match what you configured in Manage Jenkins > Tools
        jdk 'JDK17'
        maven 'Maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                // This pulls your latest code from GitHub
                checkout scm
            }
        }

        stage('Build & Test with Coverage') {
            steps {
                // Use 'bat' for Windows. Note the space between clean and test.
                bat 'mvn clean test'
            }
        }
    }

    post {
        always {
            // 1. Records the JUnit test results in the Jenkins UI
            junit '**/target/surefire-reports/*.xml'
            
            // 2. Archives the JaCoCo HTML report so you can click and view it
            archiveArtifacts artifacts: 'target/site/jacoco/**', fingerprint: true
        }
    }
}