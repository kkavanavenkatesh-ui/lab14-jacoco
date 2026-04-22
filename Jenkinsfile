pipeline {
    agent any
    tools {
        jdk 'JDK17'
        maven 'Maven3'
    }
    stages {
        stage('Build & Test with Coverage') {
            steps {
                script {
                    // This finds the base extraction folder
                    def home = tool 'JDK17'
                    
                    // We force the PATH to include the bin folder explicitly
                    withEnv(["JAVA_HOME=${home}", "PATH+JDK=${home}\\bin"]) {
                        // This 'where' command helps us debug in the logs
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
