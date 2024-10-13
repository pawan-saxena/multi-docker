pipeline {
    agent any

    stages {
        stage('Build Docker Image for Testing') {
            steps {
                script {
                    sh 'docker build -t saxenapawan800/react-test -f ./client/Dockerfile.dev ./client'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh 'docker run -e CI=true saxenapawan800/react-test npm test'
                }
            }
        }

        stage('Build Production Docker Images') {
            steps {
                script {
                    sh 'docker build -t saxenapawan800/multi-client ./client'
                    sh 'docker build -t saxenapawan800/multi-nginx ./nginx'
                    sh 'docker build -t saxenapawan800/multi-server ./server'
                    sh 'docker build -t saxenapawan800/multi-worker ./worker'
                }
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'pawan-saxena-docker-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                    }
                }
            }
        }

        stage('Push Docker Images to Docker Hub') {
            steps {
                script {
                    sh 'docker push saxenapawan800/multi-client'
                    sh 'docker push saxenapawan800/multi-nginx'
                    sh 'docker push saxenapawan800/multi-server'
                    sh 'docker push saxenapawan800/multi-worker'
                }
            }
        }
    }

    post {
        always {
            script {
                sh 'docker system prune -f'
            }
        }
    }
}
