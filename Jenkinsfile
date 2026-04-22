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
                    // Get the path where Jenkins extracted JDK17
                    def jdkHome = tool 'JDK17'
                    
                    // Force JAVA_HOME and put the NEW bin folder at the VERY START of the PATH
                    withEnv([
                        "JAVA_HOME=${jdkHome}",
                        "PATH=${jdkHome}\\bin;${env.PATH}"
                    ]) {
                        // This should now show the path inside your Jenkins workspace
                        bat 'where javac'
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
