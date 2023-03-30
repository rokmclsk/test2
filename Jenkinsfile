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
          sh "cat deployment.yaml"
          sh "sed -i 's/test.*/test:${env.BUILD_NUMBER}/g' deployment.yaml"
          sh "cat deployment.yaml"
      }
          
     stage('push the changed deployment file to github') {
          script{
               sh """
               git config --global user.name "rokmclsk"
               git config --global user.email "rokmclsk@naver.com"
               git add deployment.yaml
               git commit -m 'Update the deployment file' """
               withCredentials([usernamePassword(credentialsId: '97b00139-f330-4d78-950a-cc0bc7d0aff2', passwordVariable: 'pass', usernameVariable: 'rokmclsk')]) {
                    sh "git push http://rokmclsk:$pass@github.com/rokmclsk/test2.git master"
               }
          }
      }      
   }
}
