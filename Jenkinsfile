pipeline {
    agent any

    tools {
        // This must match the name you gave the JDK in Global Tool Configuration
        jdk 'JDK17'
        maven 'Maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test with Coverage') {
            steps {
                script {
                    // This dynamically finds the path where Jenkins extracted the JDK
                    def jdkPath = tool 'JDK17'
                    
                    // We manually set JAVA_HOME and add the bin folder to the PATH
                    withEnv(["JAVA_HOME=${jdkPath}", "PATH+JDK=${jdkPath}\\bin"]) {
                        bat 'mvn clean test'
                    }
                }
            }
        }
    }

    post {
        always {
            // Records JUnit results
            junit testResults: '**/target/surefire-reports/*.xml', allowEmptyResults: true
            
            // Archives JaCoCo reports
            archiveArtifacts artifacts: 'target/site/jacoco/**', allowEmptyArchive: true
        }
    }
}
