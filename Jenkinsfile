pipeline {
  environment {
    registry = "yang9501/swe645hw2"
    registryCredential='dockerhub'
  }
  agent any
  stages {
    stage("Building the Student Survey Image"){
      steps{
        script {
          git 'https://github.com/yang9501/SWE645HW2.git'
          sh 'pwd'
          sh 'ls'
          sh 'rm -rf *.war'
          sh 'jar -cvf SWE645HW2.war *'
          sh 'echo ${BUILD_TIMESTAMP}'
          customImage = docker.build(registry + ":$BUILD_NUMBER")
        }
      }
    }
    stage("Pushing Image to Dockerhub"){
      steps{
        docker.withRegistry('',registryCredential){
            customImage.push()
        }
      }
    }
  }
}
