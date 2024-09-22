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

        stage('Update Kubernetes Manifest') {
            steps {
                echo 'Updating manifest ...'
                script {
                    // Apply Kubernetes manifests
                    bat """

                       git push -u origin main --verbose
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
