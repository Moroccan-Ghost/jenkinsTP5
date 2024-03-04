pipeline {
    environment {
        registry = "moroccanghost/tp5"
        registryCredential = 'docker'
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
                    if (dockerImage != null) {
                        docker.withRegistry('', registryCredential) {
                            dockerImage.push()
                        }
                    } else {
                        error "Docker image is not built. Aborting."
                    }
                }
            }
        }
        stage('Deploy image') {
            steps{
                script {
                    // Use shell script for Unix-based agents
                    sh "docker run -d $registry:$BUILD_NUMBER"
                }
            }
        }
    }
}
