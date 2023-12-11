pipeline {
    agent {
        label "jenkins-agent"
    }

    environment {
        APP_NAME = "complete-prodcution-e2e-pipeline"
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

        // stage("Push changes to repo") {
        //     steps {
        //         script {
        //             def githubToken = credentials("github-token") // Use the correct environment variable for your GitHub token
        //             def gitUrl = "https://abdelhadi-ezziat:${githubToken}@github.com/Abdelhadi-ezziat/gitops-argocd-image-updater"
                    
        //             sh """
        //                 git config --global user.name "abdelhadi-ezziat"
        //                 git config --global user.email "abdelhadi.ezziat@gmail.com"
        //                 git add deployment.yaml
        //                 git commit -m "Updated deployment manifest"
        //                 git push ${gitUrl} main
        //             """
        //         }
        //     }
        // }
        stage("Push changes to repo") {
            steps {
                sh """
                    git config --global user.name "abdelhadi-ezziat"
                    git config --global user.email "abdelhadi.ezziat@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated deployment manifest"
                    git push https://abdelhadi-ezziat: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@github.com/Abdelhadi-ezziat/gitops-argocd-image-updater main
                """
                // git push https://abdelhadi-ezziat:${github-token}@github.com/Abdelhadi-ezziat/gitops-argocd-image-updater main
                // withCredentials{[gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]} {
                //     sh "git push https://github.com/Abdelhadi-ezziat/gitops-argocd-image-updater main"
                // }

            }
        }
    }
}