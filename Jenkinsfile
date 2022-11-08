pipeline {
    agent any
    environment{
         registry="275045685638.dkr.ecr.ap-south-1.amazonaws.com/test1"
         registryCredential='awsaccesskey'
         dockerImage=''
     }
     
    stages {
        stage('Initial Notification') {
            steps {
                 //put webhook for your notification channel 
                 echo 'Pipeline Start Notification'
            }
        }
       stage('Docker Build'){
            steps{
                 sh 'docker build -t test1:latest .'
                 }
        }
        stage('Docker Tagging'){
            steps{
                sh 'docker tag test1:latest 275045685638.dkr.ecr.ap-south-1.amazonaws.com/test1:${BUILD_NUMBER}'
                 }
        }
        
        
       stage('Deploy image') {
        steps{
            script{
                docker.withRegistry("https://" + registry, "ecr:ap-south-1:" + registryCredential) {
                     sh 'docker push 275045685638.dkr.ecr.ap-south-1.amazonaws.com/test1:${BUILD_NUMBER}'
                     }
            }
        }  
        }
        stage('deploy to eks'){
            steps{
            withKubeConfig(caCertificate: '', clusterName: 'new-cluster', contextName: '', credentialsId: 'ekstoken', namespace: '', serverUrl: 'https://DE8CE1B3B469E95B3DB7A78DC3FD5782.gr7.ap-south-1.eks.amazonaws.com') {
            sh 'kubectl apply -f deployment.yaml'
            sh 'kubectl set image deployments/{deploymentNmae} {container name given in deploymentyaml file}={dockerId}/{projectName}:${BUILD_NUMBER}'
            }
            }
            }
       
    }
}
    
