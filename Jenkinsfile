pipeline {
  agent any
  stages {
    stage("Building the Student Survey Image"){
      steps{
        script {
          git 'https://github.com/yang9501/SWE645HW2.git'
          sh 'rm -rf *.war'
          sh 'jar -cvf SWE645HW2.war -C WebContent/ .'
          sh 'echo ${BUILD_TIMESTAMP}'
          sh 'docker login -u yang9501 -p swe645hw2'
          def customImage = docker.build("yang9501/swe645hw2")
        }
      }
    }
  }
}
