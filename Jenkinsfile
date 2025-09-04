pipeline {
    agent any

    environment {
        AZ_API_KEY   = credentials('AZ_TOKEN')
        PROJECT_KEY  = "JjyIsSyhFlzWIxijnvYtOJpINbIFyhhl"

        GITHUB_REPOSITORY = """${
            (scm?.getUserRemoteConfigs()?.getAt(0)?.getUrl()
                ?: env.MERCURIAL_REPOSITORY_URL
                ?: env.JOB_NAME)
                .tokenize('/')
                .takeRight(2)
                .join('/')
                .replaceAll(/\.git$/, '')
                .replaceAll(/\.hg$/, '')
        }"""
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
                      --repo="$GITHUB_REPOSITORY"
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
