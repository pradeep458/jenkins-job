#!/usr/bin/env groovy
library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
        [$class: 'GitHubSCMSource',
        remote: 'https://github.com/pradeep458/shared-lib.git',
        credentialsId: 'github-credentials'])

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
