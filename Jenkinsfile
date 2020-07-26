pipeline {

  environment {
    registry = "10.132.0.2:5000/mgsgoms/flask"
    registry_mysql = "10.132.0.2:5000/mgsgoms/mysql"
    dockerImage = ""
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/poongodi1982/Docker-Project.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('current') {
      steps{
        dir("${env.WORKSPACE}/mysql"){
          sh "pwd"
          }
      }
   }
   stage('Build mysql image') {
     steps{
       sh 'docker build -t "10.132.0.2:5000/mgsgoms/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'docker push "10.132.0.2:5000/mgsgoms/mysql:$BUILD_NUMBER"'
        }
      }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kube")
        }
      }
    }

  }

}
