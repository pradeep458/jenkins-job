#!/usr/bin/env groovy
library identifier: 'shared-lib@main', retriever: modernSCM(
[$class: 'GitHubSCMSource', // Explicitly use the full class name for best practice
repoOwner: 'pradeep458',
repository: 'shared-lib',
credentialsId: 'github-credentials']) // Ensure this ID is correct

def gv

pipeline {
    agent any
    tools {
        maven 'maven-3.9'
    }
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }

        stage("build jar") {
            steps {
                script {
                    buildJar()
                }
            }
        }

        stage("build and push image") {
            steps {
                script {
                    buildImage 'pradeepmat/demo-app:jma-4.0'
                    dockerLogin()
                    dockerPush 'pradeepmat/demo-app:jma-4.0'
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
    }
}
