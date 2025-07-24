pipeline {

  environment {
    registry = "docker.io/prashanthvedarathna/flask"
    registry_mysql = "docker.io/prashanthvedarathna/mysql"
    dockerImage = ""
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/mgsgoms/Docker-Project.git'
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
       sh 'docker build -t "docker.io/prashanthvedarathna/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'docker push "docker.io/prashanthvedarathna/mysql:$BUILD_NUMBER"'
        }
      }
    stage('Deploy App to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f frontend.yaml
                kubectl get all
                '''
        }
      }
    }

  }

}
