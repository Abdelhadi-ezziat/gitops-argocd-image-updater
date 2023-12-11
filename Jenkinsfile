pipeline {
    agent {
        label "jenkins-agent"
    }

    environment {
        APP_NAME = "complete-prodcution-e2e-pipeline"
        github-token = credentials("github-token")
    }

    stages {
        stage("Cleanup workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github-token', url: 'https://github.com/Abdelhadi-ezziat/gitops-argocd-image-updater'
            }
        }

        stage("Update image tag") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }

        stage("Push changes to repo") {
            steps {
                sh """
                    git config --global user.name "abdelhadi-ezziat"
                    git config --global user.email "abdelhadi.ezziat@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated deployment manifest"
                    git push https://abdelhadi-ezziat:${github-token}@github.com/Abdelhadi-ezziat/gitops-argocd-image-updater main
                """
                // withCredentials{[gitUsernamePassword(credentialsId: 'github-token', gitToolName: 'Default')]} {
                //     sh "git push https://github.com/Abdelhadi-ezziat/gitops-argocd-image-updater main"
                // }
            }
        }
    }
}