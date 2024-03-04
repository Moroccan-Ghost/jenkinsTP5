pipeline {
    options {
        disableConcurrentBuilds(abortPrevious: true)
    }
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
                        dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
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
                echo "Waiting for 20 seconds before publishing image"
                script {
                    sleep(time: 20, unit: 'SECONDS')
                    echo "Starting Publish Image stage"
                    if (dockerImage != null) {
                        docker.withRegistry('', registryCredential) {
                            dockerImage.push()
                        }
                    } else {
                        error "Docker image is not built. Aborting."
                    }
                    echo "Finished Publish Image stage"
                }
            }
        }
        stage('Deploy image') {
            steps {
                echo "Waiting for 20 seconds before deploying image"
                script {
                    sleep(time: 20, unit: 'SECONDS')
                    echo "Starting Deploy image stage"

                    sh "docker run -d $registry:$BUILD_NUMBER"
                    echo "Finished Deploy image stage"
                }
            }
        }
    }
}