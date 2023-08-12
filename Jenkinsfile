pipeline {
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'main', url: 'https://github.com/opsgauraw/testhelmcicd.git'
             
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
                sh 'echo $VERSION'
                sh 'docker tag opsgauraw/testwebapp opsgauraw/testwebapp:$VERSION'
                //sh 'docker tag testwebapp opsgauraw/testwebapp:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
          //sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'      
       // withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
          sh  'docker login -u opsgauraw -p Opswork@022023'    
          sh  'docker push opsgauraw/testwebapp:$VERSION'
        //  sh  'docker push opsgauraw/testwebapp:latest' 
       // }
                  
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
   {
                sh "docker ps -a | grep 8085 | awk '{print \$1}' | xargs docker stop"
                sh "docker run -d -p 8092:8080 opsgauraw/testwebapp:$VERSION"
 
            }
        }
      stage('Run helm deployment') {
             
            steps {
                echo "We will try running helm deployment"
                sh "helm lint helm/myapp0412/"
                sh "helm template helm/myapp0412/"
                sh "helm upgrade --set image.tag=$VERSION myjavatest0412 helm/myapp0412/ -n helm-test"
                //sh "docker -H ssh://jenkins@172.31.28.25 run -d -p 8003:8080 opsgauraw/testwebapp"
 
            }
        }
    }
 }
