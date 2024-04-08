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
  }
}
