pipeline {
    agent any

    stages {
        stage("test") {
            steps {
                echo "Testing the application...."
                echo "Executing pipeline for branch ${BRANCH_NAME}"
            }
        }

        stage("build") {
            when {
                expression { BRANCH_NAME == "master" }
            }
            steps {
                echo "Building the application for multi-branch...."
            }
        }

        stage("deploy") {
            when {
                expression { BRANCH_NAME == "jenkins-jobs" }
            }
            steps {
                script {
                    echo "Deploying the application for multi-branch...."

                    def dockerCmd = "docker run -p 3000:8080 -d pradeepmat/demo-app:1.1.10-26"

                    sshagent(['deploy-app']) {
                        sh """
                           ssh -o StrictHostKeyChecking=no deploy-app@70.153.24.213 '${dockerCmd}'
                        """
                    }
                }
            }
        }
    }
}
