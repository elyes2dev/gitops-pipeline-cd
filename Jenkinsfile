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
                    def yamlFile = "deployment.yaml"

                    if (fileExists(yamlFile)) {
                        sh """
                            sed -i 's|image: .*|image: elyes2dev/devops-project:latest|' ${yamlFile}
                        """
                    } else {
                        error("deployment.yaml not found")
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

                        git add .
                        git commit -m "🔄 Update deployment image tag to latest"
                        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/elyes2dev/gitops-pipeline-cd ${BRANCH}
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
