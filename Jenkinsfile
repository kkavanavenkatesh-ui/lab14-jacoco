pipeline {
    agent any
    tools {
        jdk 'JDK17'
        maven 'Maven3'
    }
    stages {
        stage('Build & Test') {
            steps {
                script {
                    // 1. Get the base extraction folder
                    def jdkHome = tool 'JDK17'
                    
                    // 2. Set the environment and FORCE the path
                    // We use the full path to the bin folder to stop the Oracle error
                    withEnv([
                        "JAVA_HOME=${jdkHome}",
                        "PATH=${jdkHome}\\bin;${env.PATH}"
                    ]) {
                        // Debugging: This will show us if we successfully bypassed Oracle
                        bat 'javac -version'
                        bat 'mvn clean test'
                    }
                }
            }
        }
    }
    post {
        always {
            junit testResults: '**/target/surefire-reports/*.xml', allowEmptyResults: true
            archiveArtifacts artifacts: 'target/site/jacoco/**', allowEmptyArchive: true
        }
    }
}
