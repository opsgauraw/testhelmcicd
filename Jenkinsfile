pipeline {
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
    echo $VERSION
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
                sh 'docker tag opsgauraw/testwebapp opsgauraw/testwebapp:$VERSION'
                //sh 'docker tag testwebapp opsgauraw/testwebapp:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
          //sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'      
       // withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
          sh  'docker push opsgauraw/testwebapp:$VERSION'
        //  sh  'docker push opsgauraw/testwebapp:latest' 
       // }
                  
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
   {
                sh "docker ps -a | grep 8085 | awk '{print \$1}' | xargs docker stop"
                sh "docker run -d -p 8085:8080 opsgauraw/testwebapp:$VERSION"
 
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
    #Adding new stage of helm deployment
      stage('Run helm deployment') {
             
            steps {
                echo "We will try running helm deployment"
                sh "helm lint helm/myapp0412/"
                sh "helm template helm/myapp0412/"
                sh "helm install myjavatest0412 helm/myapp0412/ -n helm-test"
                //sh "docker -H ssh://jenkins@172.31.28.25 run -d -p 8003:8080 opsgauraw/testwebapp"
 
            }
        }
    }
 }
