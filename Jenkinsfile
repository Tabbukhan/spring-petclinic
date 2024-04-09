pipeline {
  agent any
  stages {
    stage('TruffleHog Scan') {
            steps {
                // Run TruffleHog scan
                sh 'trufflehog --regex --entropy=True .'
            }  
    }
      stage('GitGuardian Scan') {
            //agent {
                //docker { image 'gitguardian/ggshield' }
            //}
            environment {
                GITGUARDIAN_API_KEY = credentials('guardian-token')
            }
            steps {
                sh 'ggshield secret scan ci'
                //sh 'ggshield secret scan '
            }
        }
    stage('Grype Scane') {
      steps {
        sh 'grype dir:. --scope AllLayers'
      }
    }

    stage('Semgrep-Scan') {
          environment {
      // The following variable is required for a Semgrep Cloud Platform-connected scan:
      SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
          }
        steps {
          //sh 'pip3 install semgrep'
          sh 'semgrep ci'

           // Troubleshooting:

      // Uncomment the following lines if Semgrep Cloud Platform > Findings Page does not create links
      // to the code that generated a finding or if you are not receiving PR or MR comments.
       SEMGREP_JOB_URL = "${BUILD_URL}"
       SEMGREP_COMMIT = "${GIT_COMMIT}"
       SEMGREP_BRANCH = "${GIT_BRANCH}"
       SEMGREP_REPO_NAME = env.GIT_URL.replaceFirst(/^https:\/\/github.com\/(.*).git$/, '$1')
       SEMGREP_REPO_URL = env.GIT_URL.replaceFirst(/^(.*).git$/,'$1')
       SEMGREP_PR_ID = "${env.CHANGE_ID}"
      }
    }
  }
}
