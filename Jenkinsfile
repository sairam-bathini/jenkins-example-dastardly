pipeline {
  agent any
  environment {
    DASTARDLY_TARGET_URL='https://www.darinpope.com/'
  }
  stages {
    stage ("Docker Pull Dastardly from Burp Suite container image") {
      steps {
        sh 'docker pull public.ecr.aws/portswigger/dastardly:latest'
      }
    }
    stage ("Docker run Dastardly from Burp Suite Scan") {
      steps {
        cleanWs()
        sh '''
          docker run --user $(id -u) -v ${WORKSPACE}:${WORKSPACE}:rw \
          -e DASTARDLY_TARGET_URL=${DASTARDLY_TARGET_URL} \
          -e DASTARDLY_OUTPUT_FILE=${WORKSPACE}/dastardly-report.xml \
          public.ecr.aws/portswigger/dastardly:latest
        '''
      }
    }
  }
  post {
    always {
      junit testResults: 'dastardly-report.xml', skipPublishingChecks: true
    }
  }
}