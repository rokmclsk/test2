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

     stage('Deploy'){
         container('argo'){
             checkout([$class: 'GitSCM',
                      branches: [[name: '*/main' ]],
                      extensions: scm.extensions,
                      userRemoteConfigs: [[
                          url: 'git@github.com:rokmclsk/test2.git',
                          credentialsId: 'jenkins',
                        ]]
                ])
                sshagent(credentials: ['jenkins']){
                    sh("""
                        #!/usr/bin/env bash
                        set +x
                        export GIT_SSH_COMMAND="ssh -oStrictHostKeyChecking=no"
                        git config --global user.email "cure4itches@gmail.com"
                        git checkout main
                        cd env/dev && kustomize edit set image arm7tdmi/node-hello-world:${BUILD_NUMBER}
                        git commit -a -m "updated the image tag"
                        git push
                    """)
                }
            }
     }
}
}
