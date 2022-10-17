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
          //The .war file creation process breaks if there are any foreign files in the folder structure.  Have to isolate the filepath with only the bare minimum necessary in the webapps folder.
          sh 'jar -cvf SWE645HW2.war -C src/main/webapp/ .'
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
          //Jenkins uses the 'jenkins' user on the host to perform actions on the host.  .kube/config is a folder that grants access to perform kubectl actions on the cluster using the Rancher management host as a proxy, using the Rancher credentials.  I had to put the config file into a folder in the jenkins user directory, otherwise the jenkins user wouldn't have the permission to view and use the config file.
          sh 'kubectl --kubeconfig /var/lib/jenkins/.kube/config version'
          sh 'kubectl --kubeconfig /var/lib/jenkins/.kube/config set image deployment/swe645hw2 container-0=yang9501/swe645hw2:latest'
          //After the cluster receives the image, you have to redeploy the pods in order for the new image to take effect
          sh 'kubectl --kubeconfig /var/lib/jenkins/.kube/config rollout restart deployment/swe645hw2'
        }
      }
    }
  }
}
