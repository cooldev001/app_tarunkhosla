def app
pipeline {
    agent any
    tools {
        nodejs 'nodejs'
    }
    stages {
        stage('Build') {
            steps {
                echo "****Start Build Stage****"
                sh 'npm install'
                echo "****End Build Stage****"
            }
        }
        stage('Test case Execution') {
            when {
                branch 'master'
            }
            steps {
                echo "**** Start Test case Execution ****"
                sh 'npm run test'
                echo "**** End Test case Execution ****"
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
                echo "**** Start Sonarqube Analysis ****"
                withSonarQubeEnv('Test_Sonar') {
                    sh """
                        ${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=sonar-tarunkhosla -Dsonar.projectName=sonar-tarunkhosla
                    """
                }
                echo "**** End Sonarqube Analysis ****"
            }
        }
        stage('Kubernetes Deployment') {
            steps {
                echo "**** Start Kubernetes Deployment ****"
                sh """ 
                    kubectl apply -f k8s/deployment.yaml
                    kubectl apply -f k8s/service.yaml
                """
                echo "**** End Kubernetes Deployment ****"
            }
        }
    }
}
