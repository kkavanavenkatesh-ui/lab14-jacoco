pipeline {
    agent any

    tools {
        // Names must match your Jenkins Global Tool Configuration
        jdk 'JDK17'
        maven 'Maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                // Pulls the latest code from your GitHub repo
                checkout scm
            }
        }

        stage('Build & Test with Coverage') {
            steps {
                // 'bat' is used for Windows command line
                // 'mvn clean test' triggers both JUnit and JaCoCo
                bat 'mvn clean test'
            }
        }
    }

    post {
        always {
            // 1. Records JUnit results. 
            // allowEmptyResults: true prevents build failure if no tests run
            junit testResults: '**/target/surefire-reports/*.xml', allowEmptyResults: true
            
            // 2. Archives the JaCoCo HTML reports for analysis
            // allowEmptyArchive: true ensures the pipeline finishes even if reports are missing
            archiveArtifacts artifacts: 'target/site/jacoco/**', allowEmptyArchive: true
        }
    }
}
