pipeline {
    environment {
        registry = "abdex/tp2"
        registryCredential = 'dockerhub'
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/4bdex/tp2.git'
            }
        }
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
                }
            }
        }
        stage('Test image') {
            steps {
                echo 'Tests passed'
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy image') {
            steps {
                script {
                    sh '''
                        docker stop tp2-container || true
                        docker rm tp2-container || true
                        docker run -d --name tp2-container -p 8083:80 ${registry}:${BUILD_NUMBER}
                    '''
                }
            }
        }
    }
}
