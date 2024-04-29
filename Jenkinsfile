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

       
      }
    }
    stage('Check Results') {
            steps {
                script {
                    // Check the exit code to determine if secrets were found
                    int exitCode = sh(returnStatus: true, script: 'ggshield secret scan ci')
                    if (exitCode != 0) {
                        error "GitGuardian found hardcoded secrets!"
                    } else {
                        echo "No hardcoded secrets found."
                    }
                }
            }
    }
  }
}
