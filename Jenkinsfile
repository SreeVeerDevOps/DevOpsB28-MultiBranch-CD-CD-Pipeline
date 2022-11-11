//Declarative Pipeline
def VERSION='1.0.0'
pipeline {
    agent none
    // tools {
	//  maven 'apache-maven-3.6.3'
    // }
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
        stage('mvn install') {
            agent { label 'DEV' }
            steps { 
                sh "mvn install"
            }
        }
        // stage('Sonarqube SAST') {
        //     agent { label 'DEV' }
        //     steps { 
        //         sh "mvn clean verify sonar:sonar \
        //             -Dsonar.projectKey=spring-boot-app \
        //             -Dsonar.host.url=http://ec2-3-216-31-149.compute-1.amazonaws.com:9000 \
        //             -Dsonar.login=sqp_4b2278067e758dcc87471e99af4fb3cb5a3c2333"
        //     }
        // }
        stage('Sonarqube SAST') {
            agent { label 'DEV' }
            steps { 
                withSonarQubeEnv('SonarQube'){
                     sh "mvn sonar:sonar \
                     -Dsonar.projectKey=spring-boot-app \
                     -Dsonar.host.url=http://ec2-44-200-153-244.compute-1.amazonaws.com:9000"
                }

            }
        }
        stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
        stage('docker build') {
            agent { label 'DEV' }
            steps { 
                sh "sudo docker build -t sreeharshav/devopsb28spring:$BUILD_NUMBER ."
            }
        }
        stage('Deploy Docker Image') {
            agent { label 'DEV' }
            steps { 
                sh "sudo docker stop springbootapp || sudo docker ps"
                sh "sudo docker run --rm -dit --name springbootapp -p 8080:8080 sreeharshav/devopsb28spring:$BUILD_NUMBER"
            }
        }
    }
    post {
    success {
        slackSend color: good, message: "Pipeline Successfull: ${env.JOB_NAME} ${env.BUILD_NUMBER}"
    }
    failure {
        slackSend color: danger, failOnError: true, message: "SAST Failed: ${env.JOB_NAME} ${env.BUILD_NUMBER}"
    }
    always {
        echo "I always run."
    }
    }

}


