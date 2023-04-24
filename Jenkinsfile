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
              docker_image.push("$IMAGE_TAG")
              docker_image.push('latest')
            }
            }
            }
          }
          stage('Delete pushed images localy'){
          steps{
            script{
            sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
            sh "docker rmi ${IMAGE_NAME}:latest"
            }
            }
            }
            stage('Update deployment.yaml file'){
          steps{
            script{
            sh """
            cat deployment.yaml
            sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
            cat deployment.yaml
            """
            }
            }
            }
            stage('Push Deployment.yaml back to git'){
          steps{
            script{
            sh """
            git config --global user.name mukeshjava92
            git config --global user.mail mukeshjava92@gmail.com
            git add deployment.yaml
            git commit -m "Updated deployment.yaml file" 
            """
            withCredentials([string(credentialsId: 'gitcred', variable: 'gitcred')]) {
            sh 'git push "https://${gitcred}@github.com/mukeshjava92/git_argocd_minikube" main'
            }
            }
            }
            }
        }
   }