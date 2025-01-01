pipeline {
    agent any

    environment {
        SONAR_SCANNER_PATH = 'C:\\Program Files\\sonar-scanner-6.2.1.4610-windows-x64\\bin'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    def mvnHome = tool 'Maven3' // Use the Maven tool configured in Jenkins
                    withEnv(["PATH+MAVEN=${mvnHome}/bin"]) {
                        bat 'mvn clean verify'
                    }
                }
            }
        }

        stage('Sonar Analysis') {
            environment {
                SONAR_TOKEN = credentials('java-jenkins-quality') // Ensure the correct credential ID
            }
            steps {
                script {
                    def mvnHome = tool 'Maven3' // Use the Maven tool configured in Jenkins
                    withEnv(["PATH+MAVEN=${mvnHome}/bin"]) {
                        bat """
                        mvn clean verify sonar:sonar \
  -Dsonar.projectKey=java-jenkins-quality \
  -Dsonar.projectName='java-jenkins-quality' \
  -Dsonar.host.url=http://localhost:9000 \
 
                        -Dsonar.login=%SONAR_TOKEN%
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
