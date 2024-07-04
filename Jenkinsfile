pipeline{
    agent {
        label "slave"
    }
    tools{
        maven "mvn3"
        jdk "jdk17"
    }
    stages {
        stage("Workspace Cleaning") {
            steps{
                cleanWs()
            }
        }
        stage("Git Checkout") {
            steps{
                git branch: "main", credentialsId: "github", url: "https://github.com/PraveenReddy2510/complete-prodcution-e2e-pipeline.git"
            }
        }
        stage("Maven Build") {
            steps{
                sh "mvn clean package"
            }
        }
        stage("Maven Test") {
            steps{
                sh "mvn test"
            }
        }
        stage("Static Code Analysis by SonarQube") {
            steps{
                script {
                    withSonarQubeEnv(credentialsId: 'sonar') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }
        stage("Quality Gate") {
            steps{
                waitForQualityGate abortPipeline: false, credentialsId: 'sonar'
            }
        }
        stage("Docker Build and Push") {
            steps{
                script {
                    withDockerRegistry(credentialsId: 'dockerhub-token', url: 'https://hub.docker.com/') {
                    sh "docker build -t praveen4712/cicd-pipeline:1.0 ."
                    sh "docker push"
                }
                
            }
        }
    }
}