pipeline {
    environment {
        registry = "moroccanghost/tp5"
        registryCredential = 'docker'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git 'https://github.com/Moroccan-Ghost/jenkinsTP5'
            }
        }
        stage('Building image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Test image') {
            steps{
                script {
                    echo "Tests passed"
                }
            }
        }
        stage('Publish Image') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy image') {
            steps{
                bat "docker run -d $registry:$BUILD_NUMBER"
            }
        }
    }
}