pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        ACCOUNT_ID = '803039473491'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/maroofk48/Orchestration_Project.git'
            }
        }

        stage('Build Images') {
            steps {
                sh 'docker build -t frontend:v1 ./frontend'
                sh 'docker build -t hello-service:v1 ./backend/helloService'
                sh 'docker build -t profile-service:v1 ./backend/profileService'
            }
        }

       stage('ECR Login') {
    steps { 
            sh '''
                aws ecr get-login-password --region $AWS_REGION | docker login \
                --username AWS \
                --password-stdin \
                $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
            '''
        
    }
}

        stage('Tag & Push') {
            steps {
                sh '''
                docker tag frontend:v1 $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/frontend:v1
                docker push $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/frontend:v1

                docker tag hello-service:v1 $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/hello-service:v1
                docker push $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/hello-service:v1

                docker tag profile-service:v1 $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/profile-service:v1
                docker push $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/profile-service:v1
                '''
            }
        }
    }
}
