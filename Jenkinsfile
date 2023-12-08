pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Push to ECR') {
            steps {
                script {
                    // Your build script (e.g., using Dockerfile)
                    // Build and tag the Docker image
                    docker.build("your-image-name:latest")

                    // Login to AWS ECR
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'your-aws-credentials-id', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                        docker.withRegistry('https://your-account-id.dkr.ecr.your-region.amazonaws.com', 'ecr:your-region') {
                            // Push the Docker image to ECR
                            docker.image("your-image-name:latest").push()
                        }
                    }
                }
            }
        }

        stage('Deploy to ECS/Fargate') {
            steps {
                script {
                    // Your ECS deployment script
                    ecsTask([cluster: 'your-cluster-name', imageName: 'your-account-id.dkr.ecr.your-region.amazonaws.com/your-image-name:latest', launchType: 'FARGATE', taskDefinition: 'your-task-definition'])
                }
            }
        }
    }
}
