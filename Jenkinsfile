pipeline{
  agent any
  environment{
    DOCKERHUB_USERNAME = "mukeshjava92"
    APP_NAME = "git-argo-app"
    IMAGE_TAG = "${BUILD_ID}" 
    IMAGE_NAME = "$DOCKERHUB_USERNAME}"+"/"+"${APP_NAME}"
    REGISTRY_CREDS = 'dockerhub'   
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
            script{
              git
              url: 'https://github.com/mukeshjava92/git_argocd_minikube.git' 
              branch: 'main'
              }
            }
          }
        }
   }
}