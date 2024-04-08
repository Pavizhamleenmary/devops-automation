pipeline {
    agent any
    tools {
        maven 'maven 3.9.6'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Pavizhamleenmary/devops-automation']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    sh 'docker build -t kubernetesiamge .'
                }
            }
        }
        stage('Deploy to ECR') {
            steps {
                script {
                    withCredentials([
                        [
                            $class: 'AmazonWebServicesCredentialsBinding',
                            credentialsId: 'AWSCredentials',
                            accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                            secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                        ]
                    ]) {
                        sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 317143483882.dkr.ecr.ap-south-1.amazonaws.com'
                        sh 'docker tag kubernetesiamge:latest 317143483882.dkr.ecr.ap-south-1.amazonaws.com/kubernetesiamge:latest'
                        sh 'docker push 317143483882.dkr.ecr.ap-south-1.amazonaws.com/kubernetesiamge:latest'
                    }
                }
            }
        }
    }
}
