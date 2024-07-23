pipeline {
    agent any

    environment {
        REPO = 'jonathanalva92'
        IMAGE = 'my-python-app'
        DOCKER_CREDENTIALS_ID = '1234'  
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/joalvarado2/pipeline-jenkins.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${env.REPO}/${env.IMAGE}")
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'pytest tests/'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        dockerImage.push("${env.BUILD_NUMBER}")
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
