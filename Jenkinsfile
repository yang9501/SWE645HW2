pipeline {
  agent any
  stages {
    stage("Building the Student Survey Image"){
      steps{
        script {
          git 'https://github.com/yang9501/SWE645HW2.git'
          sh 'pwd'
          sh 'ls'
          sh 'rm -rf *.war'
          sh 'jar -cvf SWE645HW2.war -C webapp/ .'
          sh 'ls'
          sh 'echo ${BUILD_TIMESTAMP}'
          sh 'docker login -u yang9501 -p ${DOCKERHUB_PASS}'
          sh 'docker build -t yang9501/swe645hw2:latest .'
        }
      }
    }
    stage("Pushing Image to Dockerhub"){
      steps{
        script {
          sh 'docker push yang9501/swe645hw2:latest'
        }
      }
    }
    stage("Deploying on cluster through Rancher"){
      steps{
        script {
          sh 'kubectl --kubeconfig /var/lib/jenkins/.kube/config version'
          sh 'kubectl --kubeconfig /var/lib/jenkins/.kube/config set image deployment/swe645hw2 container-0=yang9501/swe645hw2:latest'
        }
      }
    }
  }
}
