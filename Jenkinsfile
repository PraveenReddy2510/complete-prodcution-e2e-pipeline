pipeline{
    agent {
        label "slave"
    }
    tools{
        maven "mvn3"
        jdk "jdk17"
    }
    environment{
        TAG = '1.0.' + '${BUILD_NUMBER}'
        GITHUB_CREDENTIALS = credentials('github')
    }
    stages {
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
        stage("Docker Build") {
            steps{
                script {
                    sh "docker build -t praveen4712/cicd-pipeline:${env.TAG} ."
                }
            }
        }
        stage("Docker Push") {
            steps{
                script {
                    withDockerRegistry(credentialsId: 'dockerhub-token') {
                        sh "docker push praveen4712/cicd-pipeline:${env.TAG}"
                    }
                }
            }
        }
        stage("GitOps integration with argoCD") {
            steps{
                script {
                    sh """
                    git clone https://github.com/PraveenReddy2510/complete-prodcution-e2e-pipeline-2.git
                    cd complete-prodcution-e2e-pipeline-2/CI-CD_Pipeline-1
                    git config user.email "rpravee4712@gmail.com"
                    git config user.name "Praveen"
                    
                    sed -i 's|image: praveen4712/cicd-pipeline:rathan|image: praveen4712/cicd-pipeline:${env.TAG}|g' deployment.yaml
                    
                    git add deployment.yaml
                    git commit -m "Update deployment image to with latest image"

                    git remote set-url origin https://${GITHUB_CREDENTIALS_USR}:${GITHUB_CREDENTIALS_PSW}@github.com/PraveenReddy2510/complete-prodcution-e2e-pipeline-2.git
                    git push origin main
                    """
                }
            }
        }
    }
    post{
        always{
            cleanWs()
        }
    }
}