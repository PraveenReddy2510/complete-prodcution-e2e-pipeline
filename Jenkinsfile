pipeline{
    agent {
        label "slave"
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
    }
}