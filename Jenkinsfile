pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-user-pass')
        IMAGE_NAME = "milonmiu/pyproject-20a"
        CONTAINER_NAME = "docker-cicd-container"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                    sh "docker push ${IMAGE_NAME}:latest"
                }
            }
        }
        stage('Deploy Container') {
            steps {
                script {
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
                    sh "docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${IMAGE_NAME}:latest"
                }
            }
        }
    }
    post {
        success {
            emailext(
                subject: "✅ SUCCESS: DockerHub Push & Deploy '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "অভিনন্দন ভাই! আপনার ইমেজ সফলভাবে Docker Hub-এ পুশ এবং সার্ভারে ডিপ্লয় হয়েছে।\nBuild URL: ${env.BUILD_URL}",
                to: "milon.mgi@gmail.com"
            )
        }
    }
}
