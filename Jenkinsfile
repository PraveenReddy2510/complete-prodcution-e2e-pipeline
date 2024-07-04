pipeline{
    agent {
        label "slave-1"
    }
    stages {
        stage("Workspace Cleaning") {
            steps{
                cleanWs()
            }
        }
        stage("Git Checkout") {
            steps{
                git branch: "main", credentialID: "github", url: "https://github.com/PraveenReddy2510/complete-prodcution-e2e-pipeline.git"
            }
        }
    }
}