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
                    sh 'ls -la' // List files in the workspace directory
                }
            }
        }
       
    }
}
