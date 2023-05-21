pipeline {
    agent any
    
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
                
                sh 'TAG=${IMAGE_TAG}'

                // Simple Deployment All-at-Once

                sh 'sed -i "s/weatherapp-ui.*/weatherapp-ui:${TAG}/g" kubernetes/weather-app-simple-deployment/ui-deploy.yaml'

                // Canary Deployment using istio service mesh

//                sh 'sed -i "s/weatherapp-ui.*/weatherapp-ui:${IMAGE_TAG}/g" kubernetes/weather-app-simple-deployment/ui-deployment.yaml'

                // Canary Deployment with argo rollouts and istio service mesh

//                sh 'sed -i "s/weatherapp-ui.*/weatherapp-ui:${IMAGE_TAG}/g" kubernetes/weather-app-simple-deployment/ui-deployment.yaml'

                
                withCredentials([string(credentialsId: 'github-cred', variable: 'GITHUB_TOKEN')]) {
                    sh '''
                        git config user.email "ghazouanih68@gmail.com"
                        git config user.name "Ghazouani Hichem"

                        git add .
                        git commit -m "Update deployment image to version ${TAG}"
                        git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} main
                    '''
                }
            }
        }           

        
    }
}
