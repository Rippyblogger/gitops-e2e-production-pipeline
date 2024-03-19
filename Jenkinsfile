pipeline{
    agent{
        label "jenkins-agent"
    }

    environment{
        APP_NAME = "production-e2e-pipeline"
    }

    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }

        stage("Checkout repo"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Rippyblogger/gitops-e2e-production-pipeline'
            }
        }

        stage("Update deployment tags"){
            steps{
                sh """
                cat Deployment.yaml
                sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' Deployment.yaml
                cat Deployment.yaml
                """
            }
        }

        stage("Push deployment file"){
            steps{
                sh """
                git config --global user.name "Boye"
                git config --global user.email "boyeadeotan@gmail.com"
                git add Deployment.yaml
                git commit -m "Updated Deployment manifest"
                """

                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]){
                    sh "git push https://github.com/Rippyblogger/gitops-e2e-production-pipeline main"
                }
            }
        }

    }
}