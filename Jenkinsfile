//Declarative Pipeline
def VERSION='1.0.0'
pipeline {
    agent none
    environment {
        PROJECT = "WELCOME TO DEVOPS B28 BATCH - Jenkins Class"
    }
    stages {
        stage("Tools initialization") {
            agent { label 'DEV' }
            steps {
                sh "mvn --version"
                sh "java -version"
            }
        }
        stage('mvn clean') {
            agent { label 'DEV' }
            steps { 
                sh "mvn clean"
            }
        }
        stage('mvn test') {
            agent { label 'DEV' }
            steps { 
                sh "mvn test"
            }
        }
        stage('mvn package') {
            agent { label 'DEV' }
            steps { 
                sh "mvn package"
            }
        }
    }


