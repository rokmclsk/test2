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
              
     stage('K8S Manifest Update') {
        steps {
            git credentialsId: '{jenkins}',
                url: 'https://github.com/rokmclsk/test2.git',
                branch: 'master'

            sh "sed -i 's/.*\$/${currentBuild.number}/g' deployment.yaml"
            sh "git add deployment.yaml"
            sh "git commit -m '[UPDATE] ${currentBuild.number} image versioning'"
            sshagent(credentials: ['{jenkins}']) {
                sh "git remote set-url origin git@github.com:rokmclsk/test2.git"
                sh "git push -u origin master"
             }
        }
     }
  }
}
