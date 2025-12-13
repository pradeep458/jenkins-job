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
        
        stage("build Image") {
            steps {
                script {
                    echo "Building the application...."
                    withCredentials([usernamePassword(crdentialsId:'docker-hub-repo', passwordVariables:'PASS', usernameVariables:'USER')]){
                    sh 'docker build -t pradeepmat/demo-app:jma-2.0 .'
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push pradeepmat/demo-app:jma-2.0 '
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
