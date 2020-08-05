pipeline {
  agent any
  // this is to trigger commit and merge to branch	
  environment {
    DEPLOY_NS_SIT = "jenkins-nodered"
  }

  stages {

    stage('Nodejs Install App') {
      steps {
        nodejs(nodeJSInstallationName: 'nodejs') {
                    sh 'npm --version'
        }
      }
    }

  }
   
}
