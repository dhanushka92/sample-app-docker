pipeline {
    agent any
    environment{
         registry="275045685638.dkr.ecr.ap-south-1.amazonaws.com/test"
         registryCredential='awsaccesskey'
         dockerImage=''
     }
     
    stages {
        stage('Fetching from bitbucket') {
            steps {
                git changelog: false, poll: false, url: 'https://github.com/dhanushka92/sample-app-docker.git'
            }
        }
       stage('Docker Build'){
            steps{
                 sh 'docker build -t test:latest .'
                 }
        }
        stage('Docker Tagging'){
            steps{
                 sh 'docker tag test:latest 275045685638.dkr.ecr.ap-south-1.amazonaws.com/test:latest'
                 }
        }
        
        
       stage('Deploy image') {
        steps{
            script{
                docker.withRegistry("https://" + registry, "ecr:ap-south-1:" + registryCredential) {
                     sh 'docker push 275045685638.dkr.ecr.ap-south-1.amazonaws.com/test:latest'
                     }
            }
        }  
        }
        stage('deploy to eks'){
            steps{
            withKubeConfig(caCertificate: '', clusterName: 'test-cluster', contextName: '', credentialsId: 'ekstoken', namespace: '', serverUrl: 'https://C7D00656B6894F679D76CF591A62D7B2.yl4.ap-south-1.eks.amazonaws.com') {
            sh 'kubectl apply -f deployment.yaml'
            }
            }
            }
       
    }
}
    
