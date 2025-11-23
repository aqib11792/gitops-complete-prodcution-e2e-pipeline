pipeline{
    agent{
        label "jenkins-agent"
    }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
        APP_NAME = "complete-prodcution-e2e-pipeline"
    }
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/aqib11792/gitops-complete-prodcution-e2e-pipeline'
            }

        }

        stage("Update the Deployment Tags"){
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                    
                """
            }

        }

        stage("Push the changed Deployment to GitOps Repo"){
            steps {
                sh """
                    git config --global user.name "aqib11792"
                    git config --global user.email "a.b@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([usernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/aqib11792/gitops-complete-prodcution-e2e-pipeline main"
                }
            }
        }
    }
}
