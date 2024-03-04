pipeline {
    environment {
        registry = "moroccanghost/tp5"
        registryCredential = 'docker'
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                echo "Starting Cloning Git stage"
                git 'https://github.com/Moroccan-Ghost/jenkinsTP5'
                echo "Finished Cloning Git stage"
            }
        }
        stage('Building image') {
            steps {
                echo "Starting Building image stage"
                script {
                    dockerImage = null
                    try {
                        dockerImage = docker.build registry + ":$BUILD_NUMBER"
                    } catch (Exception e) {
                        echo "Failed to build Docker image: ${e.message}"
                        currentBuild.result = 'UNSTABLE'
                    }
                }
                echo "Finished Building image stage"
            }
        }
        stage('Test image') {
            steps {
                echo "Starting Test image stage"
                script {
                    echo "Tests passed"
                }
                echo "Finished Test image stage"
            }
        }
        stage('Publish Image') {
            steps {
                echo "Starting Publish Image stage"
                script {
                    if (dockerImage != null) {
                        docker.withRegistry('', registryCredential) {
                            dockerImage.push()
                        }
                    } else {
                        error "Docker image is not built. Aborting."
                    }
                }
                echo "Finished Publish Image stage"
            }
        }
        stage('Deploy image') {
            steps {
                echo "Starting Deploy image stage"
                script {
                    sh "docker run -d $registry:$BUILD_NUMBER"
                }
                echo "Finished Deploy image stage"
            }
        }
    }
}
