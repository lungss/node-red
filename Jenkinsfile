pipeline {
  agent any
  // this is to trigger commit and merge to branch	
  environment {
    DEPLOY_NS = "jenkins-lung-nodered"
  }

  stages {

    stage('Nodejs Install App') {
      steps {
        nodejs(nodeJSInstallationName: 'nodejs') {
          sh 'npm --version'
          sh 'npm install'
          sh 'npm run build'
        }
      }
    }

    stage('Create Project') {
      steps {
        script {
          openshift.withCluster() {
    	      openshift.newProject("${DEPLOY_NS}")
          }
        }
      }
    }

    stage('Create Image Builder') {
      steps {
        script {
          openshift.withCluster() {
            openshift.withProject("${DEPLOY_NS}") {
              // openshift.newBuild("--name=nodered", "--image-stream=nodejs:latest", "--binary")
              openshift.newApp(nodered)
            }
          }
        }
      }
    }

    stage('Build Image ') {
      steps {
        script {
          openshift.withCluster() {
            openshift.withProject("${DEPLOY_NS}") {
              // openshift.selector("bc", "nodered").startBuild("--from-file=target/mapit-spring.jar", "--wait")
              def builds = openshift.selector("bc", nodered).related('builds')
              builds.untilEach(1) {
                return (it.object().status.phase == "Complete")
              }
            }
          }
        }
      }
    }

    stage('Tag Latest') {
      steps {
        script {
          openshift.withCluster() {
            openshift.withProject("${DEPLOY_NS}") {
              openshift.tag("nodered:latest")
            }
          }
        }
      }
    }

    stage('Create Deployment') {
      steps {
        script {
          openshift.withCluster() {
            openshift.withProject("${DEPLOY_NS}") {
              openshift.newApp("nodered:latest", "--name=nodered").narrow('svc').expose()
            }
          }
        }
      }
    }

  }
}
