// Full Code
  
node {
     stage('Clone repository') {
         checkout scm
     }

     stage('Build image') {
         app = docker.build("788692874122.dkr.ecr.ap-northeast-1.amazonaws.com/test")
     }

     stage('Push image') {
         sh 'rm  ~/.dockercfg || true'
         sh 'rm ~/.docker/config.json || true'
         
         docker.withRegistry('https://788692874122.dkr.ecr.ap-northeast-2.amazonaws.com', 'ecr:ap-northeast-1:5c7f7ec0-248d-4158-81b8-3ca1680e3c73') {
             app.push("${env.BUILD_NUMBER}")
             app.push("latest")
     }
  }
}
