pipeline {
    agent any
    tools {
        nodejs "node"
    }
    stages {
        stage('increment version') {
            steps {
                script {
                    dir("app") {
                        sh "npm version minor --no-git-tag-version"
                        def packageJson = readJSON file: 'package.json'
                        env.VERSION = packageJson.version?.trim()
                        echo "version updated to ${env.VERSION}"
                    }
                }
            }
        }
        stage('Run tests') {
            steps {
               script {
                    dir("app") {
                        sh "npm install"
                        sh "npm run test"
                    } 
               }
            }
        }
        stage('Build and Push docker image') {
           steps {
              withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                  sh """
                     cd app
                     docker build -t snmartins/myapp:${VERSION}-${BUILD_NUMBER} .
                     echo $PASS | docker login -u $USER --password-stdin
                     docker push snrmartins/myapp:${env.VERSION}-${BUILD_NUMBER}
                  """
                }
            }
        }         
        stage('commit version update') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'gitlab-credentials', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'
                        sh 'git remote set-url origin https://$USER:$PASS@gitlab.com/SnrMartins/java-maven-app.git'
                        sh 'git add .'
                        sh 'git commit -m "ci: version bump"'
                        sh 'git push origin HEAD:jenkins-jobs'
                    }
                }
            }
        }
    }
}