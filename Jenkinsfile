pipeline {   
    agent any
    stages {
        stage("test") {
            steps {
                script {
                    echo "Testing the application...."
                    echo "Executing pipeline for branch $BRANCH_NAME"


                }
            }
        }
        
        stage("build") {
            when{
                expression{
                    BRANCH_NAME == "master"
                }
            }
            steps {
                script {
                    echo "Building the application for multi-branch...."
                }
            }
        }

        stage("deploy") {
            when{
                expression{
                    BRANCH_NAME == "master"
                }
            }
            steps {
                script {
                    echo "Deploying the application for multi-branch...."
                }
            }
        }               
    }
} 
