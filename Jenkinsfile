pipeline {
      agent any

      environment {
          AZ_API_KEY   = credentials('AZ_TOKEN')
          PROJECT_KEY  = "jMfzJjbiOISausRwhOhvwBviWSvgEfeB"
          REPO_URL     = "${env.GIT_URL ?: env.MERCURIAL_REPOSITORY_URL ?: env.JOB_NAME}"
      }

      stages {
          stage('ArmourZero Security Test') {
              steps {
                  script {
                      def cleanBranch = env.GIT_BRANCH
                          ?.replaceFirst(/^refs\/remotes\//, '')
                          ?.replaceFirst(/^[^\/]+\//, '')

                      sh """
                          docker run --rm -v "${WORKSPACE}:/app/wrk" \
                            armourzero/pipe-scan:latest \
                            --apikey="$AZ_API_KEY" \
                            --projectkey="$PROJECT_KEY" \
                            --branch="${cleanBranch}" \
                            --repo="$REPO_URL"
                      """
                  }
              }
          }
      }

      post {
          always {
              echo "Scan completed."
          }
      }
  }
