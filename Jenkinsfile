node{
     
    stage('SCM Checkout'){
        git credentialsId: 'GIT_CREDENTIALSS', url: 'https://github.com/manikarnam/guestbook.git', branch: 'master'
    }
    
    stage('Build Docker Image'){
	    bat "docker build -t maniengg/php-redis:latest php-redis/"
    }
    stage('Build redis Docker image'){
	    bat "docker build -t maniengg/redis-follower:latest redis-follower/"
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'DOKCER_HUB_PASSWORD', variable: 'DOKCER_HUB_PASSWORD')]) {
          bat "docker login -u maniengg -p ${DOKCER_HUB_PASSWORD}"
        }
        bat "docker push maniengg/php-redis:latest"
	bat "docker push maniengg/redis-follower:latest"
     }
     
    /** stage("Deploy To Kuberates Cluster"){
       kubernetesDeploy(
         configs: 'springBootMongo.yml', 
         kubeconfigId: 'mykubeconfig',
         enableConfigSubstitution: true
        )
     }**/
	 
     stage("Deploy To Kuberates Cluster"){
        withCredentials([file(credentialsId: 'GOOGLE_APPLICATION_CREDENTIALS', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
        // sh "gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}"
        // sh "gcloud config set project mssdevops-284216"
        // sh "gcloud config set compute/zone us-central1-c"
        // sh "gcloud config set compute/region us-central1"
        // sh "gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project mssdevops-284216"
         //sh "sed -i -e 's,image_to_be_deployed,'maniengg/spring-boot-mongo:${BUILD_ID}',g' springBootMongo.yml"
         bat "kubectl apply -f frontend-deployment.yaml"      
         bat "kubectl apply -f frontend-service.yaml"
	 bat "kubectl apply -f redis-follower-deployment.yaml"
         bat "kubectl apply -f redis-follower-service.yaml"
	 bat "kubectl apply -f redis-leader-deployment.yaml"
	 bat "kubectl apply -f redis-leader-service.yaml"
        }
      }
}
