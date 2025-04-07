pipeline {
    agent any

    tools {
        jdk 'jdk-17'
        gradle 'gradle-8.13'
    }

    environment {
        SONAR_SCANNER_HOME = tool 'SonarQubeScanner'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/shivamBhavsar-cmd/jenkins-test-1.git'
            }
        }

        stage('Build') {
            steps {
                sh '${GRADLE_HOME}/bin/gradle build'
            }
        }

        stage('Test') {
            steps {
                sh 'gradle test'
            }
        }

        stage('Code Quality - Checkstyle') {
            steps {
                sh 'gradle check'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "${SONAR_SCANNER_HOME}/sonar-scanner"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
