pipeline {
    agent any
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/opsgauraw/testcicd.git'
             
          }
        }
  stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
   stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t opsgauraw/testwebapp .' 
                sh 'docker tag opsgauraw/testwebapp opsgauraw/testwebapp:latest'
                //sh 'docker tag testwebapp opsgauraw/testwebapp:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
          //sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'      
       // withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
          sh  'docker push opsgauraw/testwebapp:latest'
        //  sh  'docker push opsgauraw/testwebapp:latest' 
       // }
                  
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
   {
                sh "docker ps -a | grep 8085 | awk '{print \$1}' | xargs docker stop"
                sh "docker run -d -p 8085:8080 opsgauraw/testwebapp:latest"
 
            }
        }
 stage('Run Docker container on Kubernetes') {
             
            steps {
                echo "We will try this also"
                sh "kubectl delete pod \$(kubectl get pods | grep tomcat | awk '{print \$1}')"
                sh "kubectl apply -f kubernetes.yml"
                sh "kubectl apply -f service-definition.yml"
                //sh "docker -H ssh://jenkins@172.31.28.25 run -d -p 8003:8080 opsgauraw/testwebapp"
 
            }
        }
    }
 }
