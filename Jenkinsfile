pipeline {
    agent { label "Jenkins-Agent" }
    environment {
        GIT_REPO = 'https://github.com/elyes2dev/gitops-pipeline-cd.git'
        BRANCH = 'main'
    }

    stages {
        stage('Clone GitOps Repo') {
            steps {
                git branch: "${BRANCH}", credentialsId: 'github', url: "${GIT_REPO}"
            }
        }

        stage('Update Deployment Files') {
            steps {
                script {
                    def files = ['mysql-deploy.yaml', 'frontend-deploy.yaml', 'backend-deploy.yaml']

                    files.each { file ->
                        if (fileExists(file)) {
                            echo "Updating image in ${file}"
                            if (file == 'mysql-deploy.yaml') {
                                // usually mysql doesn't need an image update, but if needed, add here
                                sh """
                                    # Example: update mysql image tag if needed
                                    # sed -i 's|image: mysql:.*|image: mysql:8.0|' ${file}
                                """
                            } else if (file == 'frontend-deploy.yaml') {
                                sh """
                                    sed -i 's|image: .*|image: elyeshub/portfolio-app-pipeline-frontend:${IMAGE_TAG}|' ${file}
                                """
                            } else if (file == 'backend-deploy.yaml') {
                                sh """
                                    sed -i 's|image: .*|image: elyeshub/portfolio-app-pipeline-backend:${IMAGE_TAG}|' ${file}
                                """
                            }
                        } else {
                            error("File not found: ${file}")
                        }
                    }
                }
            }
        }

        stage('Commit & Push Changes') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh """
                        git config --global user.name "elyes2dev"
                        git config --global user.email "elyes.zoghlami.1@esprit.tn"

                        git add mysql-deploy.yaml frontend-deploy.yaml backend-deploy.yaml
                        git commit -m "🔄 Update deployment image tags to ${IMAGE_TAG}"
                        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/elyes2dev/gitops-pipeline-cd.git ${BRANCH}
                    """
                }
            }
        }
    }

    post {
        success {
            echo '✅ CD pipeline executed successfully.'
        }
        failure {
            echo '❌ CD pipeline failed.'
        }
    }
}
