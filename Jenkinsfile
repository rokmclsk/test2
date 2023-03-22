node {   
     // git 연동
     stage('Clone repository') { 
         checkout scm
     }     
     
     // 도커 이미지 build
     stage('Build image') { 
         app = docker.build("788692874122.dkr.ecr.ap-northeast-1.amazonaws.com/test")
     }
     
     // ecr repo 에 도커 이미지 push
     stage('Push image') {  
         sh 'rm -f ~/.dockercfg ~/.docker/config.json || true'
         
         docker.withRegistry('YOUR_REGISTRY', 'YOUR_CREDENTIAL'){
             app.push("${env.BUILD_NUMBER}")
         }         
     }

     // updated docker image 태그를 git push 
     stage('Deploy') { 
         // 사전 준비
         sh("""
            git config --global user.name "Your Name"
            git config --global user.email "you@example.com"
            git checkout -B master
         """)
      withCredentials([usernamePassword(credentialsId: 'github-signin', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {   
            sh("""
               #!/usr/bin/env bash
               git config --local credential.helper "!f() { echo username=\\$GIT_USERNAME; echo password=\\$GIT_PASSWORD; }; f"
               cd prod && kustomize edit set image 788692874122.dkr.ecr.ap-northeast-1.amazonaws.com/test:${BUILD_NUMBER}
               git add kustomization.yaml
               git status
               git commit -m "update the image tag"
               git push origin HEAD:master
            """)   
      }
         
     }
          
}
