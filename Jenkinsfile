node {
     stage('Clone repository') {
         checkout scm
     }

     stage('Build image') {
         app = docker.build("788692874122.dkr.ecr.ap-northeast-1.amazonaws.com/test")
     }

     stage('Push image') {
         docker.withRegistry('https://788692874122.dkr.ecr.ap-northeast-1.amazonaws.com', 'ecr:ap-northeast-1:ecr-credential') {
             app.push("${env.BUILD_NUMBER}")
             app.push("latest")
     }  

     stage('updating kubernetes deployment file') {
          sed -i 's/test.*/test:${env.BUILD_NUMBER}/g' deployment.yaml"
      }
   }
}
