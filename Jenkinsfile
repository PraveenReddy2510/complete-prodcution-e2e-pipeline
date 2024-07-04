pipeline{
    agent {
        label "slave"
    }
    tools{
        maven ""
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
    }
}