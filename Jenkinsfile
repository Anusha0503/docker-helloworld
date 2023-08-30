node {
          stage ('checkout'){
           git branch: 'master', credentialsId: 'gitcredentials', url: 'https://github.com/Anusha0503/mvn-web-app3.git'
 
         }
   stage ('build'){ 
        withMaven(globalMavenSettingsConfig: '', jdk: 'java', maven: 'maven', mavenSettingsConfig: '', traceability: true) {
        sh 'mvn clean package'
                  }
              }
    stage ('docker build image'){
      sh " docker build -t mvn-web-app3:v1 ."
    }
     stage ('docker tag&Push image'){
               sh " docker login -u mydocker1405 -p Password@123 "
               sh " docker tag mvn-web-app3:v1 mydocker1405/mvn-web-app3:$BUILD_NUMBER "
               sh  " docker push mydocker1405/mvn-web-app3:$BUILD_NUMBER "
     }
     stage (' deploy to k8s'){
     sshagent(['kubernetes_pem']) {
               sh " scp -o stricthostkeychecking=no mvn-web-app3-deployment.yaml ubuntu@54.91.203.20:/home/ubuntu"
               sh " scp -o stricthostkeychecking=no mvn-web-app3-service.yaml ubuntu@54.91.203.20:/home/ubuntu"        
               sh " ssh ubuntu@54.91.203.20  kubectl apply -f mvn-web-app3-deployment.yaml"
               sh " ssh ubuntu@54.91.203.20  kubectl apply -f mvn-web-app3-service.yaml"
          }
       }
     }
 
