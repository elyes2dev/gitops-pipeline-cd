pipeline {
    agent { label "Jenkins-Agent" }
    environment {
        APP_NAME = "portfolio-app"
        REPO_URL = "https://github.com/elyes2dev/gitops-pipeline-cd"
        GIT_CRED_ID = "github"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: "${GIT_CRED_ID}", url: "${REPO_URL}"
            }
        }

        stage("Update Deployment Tags") {
            steps {
                sh """
                    for file in mysql-deploy.yaml backend-deploy.yaml frontend-deploy.yaml; do
                      echo "Updating \$file with IMAGE_TAG=${IMAGE_TAG}"
                      sed -i 's|image:.*|image: ${APP_NAME}:${IMAGE_TAG}|' \$file
                      cat \$file
                    done
                """
            }
        }


        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "elyes2dev"
                   git config --global user.email "elyes.zoghlami.1@esprit.tn"
                   git add .
                   git commit -m "Updated Deployment Manifests"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/elyes2dev/gitops-pipeline-cd main"
                }
            }
        }
      
    }
}
