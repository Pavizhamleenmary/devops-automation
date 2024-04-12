pipeline {
    agent any
    tools {
        maven 'maven 3.9.6'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Pavizhamleenmary/devops-automation']]])
                sh 'mvn clean package' // Package the application into a JAR file
                script {
                    // Navigate into the target directory
                    dir('target') {
                        // Find the JAR file dynamically
                        def jarFileName = sh(script: 'ls *.jar', returnStdout: true).trim()
                        
                        // Copy the JAR file to S3 bucket
                        withCredentials([
                            [
                                $class: 'AmazonWebServicesCredentialsBinding',
                                credentialsId: 'AWSCredentials',
                                accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                                secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                            ]
                        ]) {
                            sh "aws s3 cp ${jarFileName} s3://citbucketlatest/"
                        }
                    }
                }
            }
        }
        // Other stages here...
    }
}
