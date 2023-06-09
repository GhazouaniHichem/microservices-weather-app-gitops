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

                // Simple Deployment All-at-Once using istio service mesh

                sh 'sed -i "s/weatherapp-ui.*/weatherapp-ui:${IMAGE_TAG}/g" kubernetes/weather-app-simple-deployment/ui-deploy.yaml'

                // Canary Deployment using istio service mesh

                sh 'sed -i "s/weatherapp-ui.*/weatherapp-ui:${IMAGE_TAG}/g" kubernetes/weather-app-istio-deployment/ui-deploy-v2.yaml'

                // Canary Deployment with argo rollouts and istio service mesh

                sh 'sed -i "s/weatherapp-ui.*/weatherapp-ui:${IMAGE_TAG}/g" kubernetes/weather-app-argo-rollout-deployment/ui-ms-rollout-deployment.yaml'

                
                withCredentials([string(credentialsId: 'github-cred', variable: 'GITHUB_TOKEN')]) {
                    sh '''
                        git config user.email "ghazouanih68@gmail.com"
                        git config user.name "Ghazouani Hichem"

                        git add .
                        git commit -m "Update deployment image to version ${IMAGE_TAG}"
                        git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} main
                    '''
                }
            }
        }           

        
    }
}
