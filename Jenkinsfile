pipeline {
    agent {
        docker {
            image 'node:6-alpine' 
        }
    }
    environment {
        CI = 'true' 
    }
    stages {
      stage("Build") {
        steps {
          sh 'npm install'
        }
      }
      stage("Running the test suite") {
        steps {
          sh 'npm t'
        }
      }
      stage("Bundling") {
        steps {
          sh 'npm run prod'
        }
      }
       stage('Release') {
             steps {
                 withAWS(region:'eu-central-1',credentials:'aws-static') {
                 sh 'echo "Uploading content to AWS S3"'
                     s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, folder:'dist', bucket:'static-jenkins-pipeline')
                 }
             }
        }
    }
    post {
      success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
    }
}