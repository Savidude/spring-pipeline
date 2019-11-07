pipeline {
    environment {
        registry = "savidude/spring-boot-test"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    agent any
    tools {
        maven 'apache-maven-3.0.1' 
    }

    options {
        skipStagesAfterUnstable()
    }

    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') { 
            steps{
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy Image') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }


}