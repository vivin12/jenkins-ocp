pipeline {

  environment {
    imageName = "default-route-openshift-image-registry.cloud/demo/spring-app"
    registryCredential = 'ocp-sandbox'
    imageVersion = ''
  }

  agent any
  stages {
      stage('Git-clone') {
        steps {
            git 'https://github.com/vivin12/test-app'
        }
    }

    stage('Build-Image') {
      steps {
        script {
          imageVersion = docker.build imageName + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push-Image') {
      steps {
        script {
            docker.withRegistry('https://default-route-openshift-image-registry.cloud/demo/spring-app', registryCredential) {
            imageVersion.push('latest')
          }
        }
      }
    }
    
    stage('Deploy-App') {
      steps {
        script {
           sh 'oc apply -f ocp/deployment.yaml'
           sh 'oc apply -f ocp/service.yaml'
           sh 'oc apply -f ocp/route.yaml'
           //sh 'oc apply -f dc.yaml'
          }
        }
      }
  }
}
