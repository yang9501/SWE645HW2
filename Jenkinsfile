pipeline {
  agent any
  environment {

  }
  stages {
    stage("Building the Student Survey Image"){
      steps{
        script {
          checkout scm
          sh 'rm -rf *.war'
          sh 'jar -cvf SWE645HW2.war -C WebContent/ .'
          sh 'echo ${BUILD_TIMESTAMP}'
          sh 'docker login -u yang9501 -p ${DOCKERHUB_PASS}"
          def customImage = docker.build("yang9501/swe645hw2:latest")
        }
      }
    }
  }
}
