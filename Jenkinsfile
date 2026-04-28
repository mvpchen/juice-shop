pipeline {
      agent any

      environment {
          AZ_API_KEY   = credentials('WM_AZ_TOKEN')
          PROJECT_KEY  = "JjyIsSyhFlzWIxijnvYtOJpINbIFyhhl"
          REPO_URL     = "${env.GIT_URL ?: env.MERCURIAL_REPOSITORY_URL ?: env.JOB_NAME}"
      }

      stages {
          stage('ArmourZero Security Test') {
              steps {
                  script {
                      def cleanBranch = env.BRANCH_NAME ?: env.GIT_BRANCH
                          ?.replaceFirst(/^refs\/remotes\//, '')
                          ?.replaceFirst(/^[^\/]+\//, '')

                      withEnv(["CLEAN_BRANCH=${cleanBranch}"]) {
                          sh '''
                              docker run --rm -v "${WORKSPACE}:/app/wrk" \
                                armourzero/pipe-scan:latest \
                                --apikey="$AZ_API_KEY" \
                                --projectkey="$PROJECT_KEY" \
                                --branch="$CLEAN_BRANCH" \
                                --repo="$REPO_URL"
                          '''
                      }
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
