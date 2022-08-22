def app
pipeline {
    agent any
    tools {
        nodejs 'nodejs'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test case Execution') {
            when {
                branch 'master'
            }
            steps {
                sh 'npm run test'
            }
        }
        stage('Sonarqube Analysis') {
            when {
                branch 'develop'
            }
            environment {
                scannerHome = tool 'SonarQubeScanner'
            }
            steps {
                withSonarQubeEnv('Test_Sonar') {
                    sh """
                        ${scannerHome}/bin/sonar-scanner" /
                        -Dsonar.projectKey=sonar-tarunkhosla \
                        -Dsonar.projectName=sonar-tarunkhosla
                    """
                }
            }
        }
        stage('Kubernetes Deployment') {
            steps {
                
                sh """ 
                    kubectl apply -f k8s/deployment.yaml
                    kubectl apply -f k8s/service.yaml
                """
            }
        }
    }
}
