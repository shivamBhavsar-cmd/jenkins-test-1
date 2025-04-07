pipeline {
    agent any

    tools {
        gradle 'Gradle-8.13'  
        jdk 'Java-17'         
    }

    environment {
        SONARQUBE_SERVER = 'SonarQube' 
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh '${GRADLE_HOME}/bin/gradle build'
            }
        }

        stage('Test') {
            steps {
                sh '${GRADLE_HOME}/bin/gradle test'
            }
        }

        stage('Code Quality - Checkstyle') {
            steps {
                sh '${GRADLE_HOME}/bin/gradle check'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    sh "${GRADLE_HOME}/bin/gradle sonarqube"
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

        stage('Debug') {
            steps {
                sh 'echo GRADLE_HOME is $GRADLE_HOME'
                sh 'ls -l $GRADLE_HOME/bin'
                sh '$GRADLE_HOME/bin/gradle --version'
            }
        }
    }
}
