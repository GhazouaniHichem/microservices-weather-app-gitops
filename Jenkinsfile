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
                git credentialsId: 'github', branch: 'main', changelog: false, poll: false, url: 'https://github.com/GhazouaniHichem/microservices-weather-app-gitops.git'
            }
        }
        
        
        
        stage('Update Deployment File') {
            environment {
                GIT_REPO_NAME = "microservices-weather-app-gitops"
                GIT_USER_NAME = "GhazouaniHichem"
            }
            steps {
                withCredentials([string(credentialsId: 'github-cred', variable: 'GITHUB_TOKEN')]) {
                    sh '''
                        git config user.email "ghazouanih68@gmail.com"
                        git config user.name "Ghazouani Hichem"
                        BUILD_NUMBER=${IMAGE_TAG}
                        sed -i "s/weatherapp-ui.*/weatherapp-ui:${IMAGE_TAG}/g" kubernetes/ui-deployment.yaml
                        git add .
                        git commit -m "Update deployment image to version ${IMAGE_TAG}"
                        git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} main
                    '''
                }
            }
        }           

        
    }
}
