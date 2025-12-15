def gv

pipeline {
    agent any
    tools {
        maven 'maven-3.9'
    }
    stages {
        stage('increment version') {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }
        stage('build app') {
            steps {
                script {
                    echo 'building the application...'
                    sh 'mvn clean package'
                }
            }
        }
        stage('build image') {
            steps {
                script {
                    echo "building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]){
                        sh "docker build -t pradeepmat/demo-app:${IMAGE_NAME} ."
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh "docker push pradeepmat/demo-app:${IMAGE_NAME}"
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    echo 'deploying docker image...'
                }
            }
        }
        stage('commit version update') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'git-commit-token', variable: 'GITHUB_TOKEN')]) {
        
                        sh '''
                          set -e
        
                          git config user.email "jenkins@example.com"
                          git config user.name "jenkins"
        
                          echo "Git status before commit:"
                          git status
        
                          # Detached HEAD is normal in Jenkins
                          git branch -a
        
                          # Use PAT safely (no username, no URL breakage)
                          git remote set-url origin https://x-access-token:${GITHUB_TOKEN}@github.com/pradeep458/jenkins-job.git
        
                          git add .
                          git commit -m "ci: version bump" || echo "Nothing to commit"
        
                          git push origin HEAD:jenkins-jobs
                        '''
                    }
                }
            }
        }
    }
}
        
          
