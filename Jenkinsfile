pipeline {
    agent any

    environment {
        AZ_API_KEY   = credentials('AZ_TOKEN')
        PROJECT_KEY  = "JjyIsSyhFlzWIxijnvYtOJpINbIFyhhl"
        REPO_URL     = "${env.GIT_URL ?: env.MERCURIAL_REPOSITORY_URL ?: env.JOB_NAME}"
    }

    stages {
        stage('ArmourZero Security Test') {
            steps {
                sh '''
                    docker run --rm -v "${WORKSPACE}:/app/wrk" \
                      armourzero/pipe-scan:latest \
                      --apikey="$AZ_API_KEY" \
                      --projectkey="$PROJECT_KEY" \
                      --branch="$GIT_BRANCH" \
                      --repo="$REPO_URL"
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline completed."
        }
    }
}
