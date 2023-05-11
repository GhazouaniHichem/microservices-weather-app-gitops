pipeline {
    agent any
    
    tools {
        jdk 'jdk11'
        maven 'maven3'
    }
    
   

    stages {
         stage('Cleanup Workspace'){
            steps {
                script {
                    cleanWs()
                }
            }
        }


        stage('Git Checkout ') {
            steps {
                git credentialsId: 'github', branch: 'main', changelog: false, poll: false, url: 'https://github.com/GhazouaniHichem/MERN-app-CI-CD-gitops.git'
            }
        }
        
        
        
        stage('Update Deployment File') {
            environment {
                GIT_REPO_NAME = "MERN-app-CI-CD-gitops"
                GIT_USER_NAME = "GhazouaniHichem"
            }
            steps {
                withCredentials([string(credentialsId: 'github-cred', variable: 'GITHUB_TOKEN')]) {
                    sh '''
                        git config user.email "ghazouanih68@gmail.com"
                        git config user.name "Ghazouani Hichem"
                        BUILD_NUMBER=${BUILD_NUMBER}
                        sed -i "s/movies-mern-app-frontend.*/movies-mern-app-frontend:${IMAGE_TAG}/g" kubernetes/frontend-deployment.yaml
                        sed -i "s/movies-mern-app-backend.*/movies-mern-app-backend:${IMAGE_TAG}/g" kubernetes/server-deployment.yaml
                        git add .
                        git commit -m "Update deployment image to version ${IMAGE_TAG}"
                        git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} main
                    '''
                }
            }
        }           

        
    }
}
