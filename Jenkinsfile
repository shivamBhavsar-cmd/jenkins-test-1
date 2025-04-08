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
                script {
                    def gradleHome = tool 'Gradle-8.13'
                    withEnv(["PATH+GRADLE=${gradleHome}/bin"]) {
                        sh 'gradle build'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    def gradleHome = tool 'Gradle-8.13'
                    withEnv(["PATH+GRADLE=${gradleHome}/bin"]) {
                        sh 'gradle test'
                    }
                }
            }
        }

        // stage('Code Quality - Checkstyle') {
        //     steps {
        //         script {
        //             def gradleHome = tool 'Gradle-8.13'
        //             withEnv(["PATH+GRADLE=${gradleHome}/bin"]) {
        //                 sh 'gradle check'
        //             }
        //         }
        //     }
        // }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def gradleHome = tool 'Gradle-8.13'
                    withEnv(["PATH+GRADLE=${gradleHome}/bin"]) {
                        withSonarQubeEnv("${SONARQUBE_SERVER}") {
                            sh 'gradle sonarqube'
                        }
                    }
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
                script {
                    def gradleHome = tool 'Gradle-8.13'
                    withEnv(["PATH+GRADLE=${gradleHome}/bin"]) {
                        sh 'echo GRADLE_HOME is $PATH'
                        sh 'gradle --version'
                    }
                }
            }
        }
    }
}
