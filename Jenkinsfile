pipeline {
  agent any
  // this is to trigger commit and merge to branch	
  environment {
    DEPLOY_NS_SIT = "jenkins-nodered"
  }

  stages {

    stage('Nodejs Install App') {
      steps {
        sh 'npm --version'
      }
    }

  }
   
}	
