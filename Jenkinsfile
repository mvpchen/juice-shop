pipeline {
    agent any

    environment {
        AZ_API_KEY   = credentials('AZ_TOKEN')
        PROJECT_KEY  = "JjyIsSyhFlzWIxijnvYtOJpINbIFyhhl"
        GIT_URL = scm.getUserRemoteConfigs()[0].getUrl()
        GITHUB_REPOSITORY = "${GIT_URL.tokenize('/').takeRight(2).join('/').replaceAll(/\.git$/, '')}"
    }

    stages {
        stage('ArmourZero Security Test') {
            steps {
                script {
                    sh 'echo "Running security scan for branch: $GIT_BRANCH"'

                    // Debug: list files in the workspace
                    sh 'echo "Workspace files:" && ls -alh'

                    // Optional: list inside the mounted path in the Docker container
                    sh '''
                        echo "Files in /app/wrk inside container:"
                        docker run --rm -v "$(pwd):/app/wrk" alpine ls -alh /app/wrk
                    '''

                    // Run your scan
                    sh '''
                        docker run --rm -v "$(pwd):/app/wrk" \
                          armourzero/pipe-scan:latest \
                          --apikey="$AZ_API_KEY" \
                          --projectkey="$PROJECT_KEY" \
                          --branch="$GIT_BRANCH" \
                          --repo="$GITHUB_REPOSITORY"
                    '''
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline completed."
        }
    }
}
