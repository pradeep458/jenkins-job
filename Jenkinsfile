pipeline {   
    agent any
    tools{
        maven 'maven-3.9'
    }
    stages {
        stage("build jar") {
            steps {
                script {
                    echo "building app"
                    sh 'mvn package'
                }
            }
        }
        
        stage("Build Image") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    // All commands that need the credentials must be inside this block
                    script {
                        echo "Building the application...."
                        sh 'docker build -t pradeepmat/demo-app:jma-2.0 .'
                        
                        // Use the variables $PASS and $USER here
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh 'docker push pradeepmat/demo-app:jma-2.0'
                    }
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    echo "Deploying the application...."
                }
            }
        }               
    }
} 
