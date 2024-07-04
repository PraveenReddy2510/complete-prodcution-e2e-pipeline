pipeline{
    agent {
        label "slave-1"
    }
    stages {
        stage("Workspace Cleaning") {
            step{
                cleanWs()
            }
        }
    }
}