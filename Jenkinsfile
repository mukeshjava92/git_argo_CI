pipeline{
  agent any
  environment{
    DOCKER_HUB_USERNAME = "mukeshjava92"
    APP_NAME = "git-argo-app"
    IMAGE_TAG = "${BUILD_ID}" 
    IMAGE_NAME = "${DOCKER_HUB_USERNAME}"+"/"+"${APP_NAME}"
    REGISTRY_CREDS = 'dockercred'   
  }
  stages{

        stage( 'Clear the Work Space'){
          steps{
            script{
              cleanWs()
            }
          }
        }
        stage('Checkout SCM'){
          steps{
            git branch: 'main', url: 'https://github.com/mukeshjava92/git_argocd_minikube.git'
            }
          }
        stage('Docker image build'){
          steps{
            script{
            docker_image = docker.build "${IMAGE_NAME}"
            }
            }
          }
          stage('Push image to Dockerhub'){
          steps{
            script{
            docker.withRegistry('',REGISTRY_CREDS){
              docker_image.push("$BUILD_NUMBER")
              docker_image.push('latest')
            }
            }
            }
          }
          stage('Delete pushed images localy'){
          steps{
            script{
            sh "docker rmi ${IMAGE_NAME}:${BUILD_NUMBER}"
            sh "docker rmi ${IMAGE_NAME}:latest"
            }
            }
            }
        }
   }