pipeline {
    agent any
    environment {
        IMAGE_NAME = "docker-cicd-demo"
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
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }
        stage('Stop Existing Container') {
            steps {
                script {
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    sh "docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
                }
            }
        }
    }
    post {
        success {
            emailext(
                subject: "✅ SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "অভিনন্দন ভাই! আপনার Docker CI/CD পাইপলাইন সফলভাবে রান করেছে।\nBuild URL: ${env.BUILD_URL}",
                to: "milon.mgi@gmail.com"
            )
        }
        failure {
            emailext(
                subject: "❌ FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "সতর্কতা! আপনার পাইপলাইন বিল্ড ফেইল করেছে। লগ চেক করুন:\nBuild URL: ${env.BUILD_URL}",
                to: "milon.mgi@gmail.com"
            )
        }
    }
}
