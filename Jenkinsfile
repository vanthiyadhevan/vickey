pipeline {
    agent any

    // tools {
    //     git 'gitcli'
    // }
    environment {
        GITHUB_TOKEN = credentials('github') // GitHub token as Jenkins credential
        GITHUB_REPO = credentials('repo_url') // Repository URL as Jenkins credential
        REPO_OWNER = credentials('repo_owner')
        REPO_NAME = credentials('repo_name') // vickey

    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/vanthiyadhevan/vickey.git', branch: 'staging'
            }
        }

        stage('Create PR from staging to main') {
            // steps {
            //     script {
            //         // Ensure GitHub CLI is installed and authenticated
            //         sh '''
            //             echo "Authenticating GitHub CLI..."
            //             sudo apt-get update && sudo apt-get install gh -y
            //             echo $GITHUB_TOKEN | gh auth login --with-token

            //             # Check if authentication succeeded
            //             gh auth status || exit 1  # If authentication fails, exit with error

            //             echo "Creating pull request from staging to main..."
            //             gh pr create \
            //               --repo "$GITHUB_REPO" \
            //               --base main \
            //               --head staging \
            //               --title "Merge staging into main" \
            //               --body "Automated PR from Jenkins pipeline"
            //         '''
            //     }
            // }
            steps {
                sh """
                    echo "Creating PR via GitHub API..."

                    curl -X POST https://api.github.com/repos/$REPO_OWNER/$REPO_NAME/pulls \\
                        -H "Authorization: token $GITHUB_TOKEN" \\
                        -H "Accept: application/vnd.github.v3+json" \\
                        -d '{
                            "title": "from staging to main",
                            "head": "staging",
                            "base": "main",
                            "body": "Updated docker image in deployment manifest"
                        }'
                    """
                }
            }
        }

    post {
        always {
            slackSend channel: '#jenkins-cicd',
            message: "Find Status of Pipeline: ${currentBuild.currentResult} ${env.JOB_NAME} ${env.BUILD_NUMBER} ${BUILD_URL}"
        }
    }
}
