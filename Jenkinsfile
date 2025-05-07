pipeline {
	agent any

    env {
        GITHUB_TOKEN = credentials('github')
        GITHUB_REPO = credentials('repo_url')
    }

	stages {
        stage('checkout') {
            steps{
                git url:"https://github.com/vanthiyadhevan/vickey.git", branch: "staging"
            }
        }
        stage('Create PR from staging to main') {
            steps {
                sh '''
                    echo "Authenticating GitHub CLI..."
                    echo $GITHUB_TOKEN | gh auth login --with-token

                    echo "Creating pull request from staging to main..."
                    gh pr create \
                      --repo "$GITHUB_REPO" \
                      --base main \
                      --head staging \
                      --title "Merge staging into main" \
                      --body "Automated PR from Jenkins pipeline"
                '''
            }
        }
        
    }
    post {
        always {
            //Add channel name
            slackSend channel: 'jenkins-cicd',
            message: "Find Status of Pipeline:- ${currentBuild.currentResult} ${env.JOB_NAME} ${env.BUILD_NUMBER} ${BUILD_URL}"
        }
    }
}