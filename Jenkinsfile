pipeline {
    agent any
    environment {
        // Replace these with your Docker Hub credentials and repository info
        DOCKER_HUB_CREDENTIALS = 'aba091eb-3857-489f-8115-2993e248f42c'
        IMAGE_TAG = 'aashraf756/springboot-user-service'
        IMAGE_VERSION = "v1.2" // or use env.BUILD_NUMBER or another unique identifier
        MANIFEST_REPO = "https://github.com/anas-ash99/manifest"
        MANIFEST_REPO_NAME = "manifest"
        DEPLOYMENT_FILE_PATH = "overlys\\dev\\user-service"
    }

    stages {
        stage('Build App') {
            steps {
                echo 'Building the app ...'
                bat 'mvnw.cmd clean package'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    bat "docker build -t ${IMAGE_TAG}:${IMAGE_VERSION} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_HUB_CREDENTIALS) { // login into dockerhub
                        echo 'Pushing docker image...'
                        bat "docker push ${IMAGE_TAG}:${IMAGE_VERSION}"
                    }
                }
            }
        }

        stage('Update Kubernetes Manifest') {
            steps {
                echo 'Updating manifest ...'
                script {
                    // Apply Kubernetes manifests
                    bat """
                       git config --global user.email "anas.ash099@example.com"
                       git config --global user.name "Anas Ashraf"
                       git pull
                       cd ${MANIFEST_REPO_NAME}
                       powershell -Command "(Get-Content -Path '${DEPLOYMENT_FILE_PATH}\\deployment.yaml') -replace '${IMAGE_TAG}:.*', '${IMAGE_TAG}:${IMAGE_VERSION}' | Set-Content -Path '${DEPLOYMENT_FILE_PATH}\\deployment.yaml'"
                       git add .
                       git commit -m "update tag image by Jenkins"
                       git push -u origin main
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
        }
        success {
            echo 'Build and deployment succeeded!'
        }
        failure {
            echo 'Build or deployment failed.'
        }
    }
}
