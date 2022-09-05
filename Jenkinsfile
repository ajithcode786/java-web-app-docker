node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/ajithcode786/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t kingajith/java-web-app .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'docicd', variable: 'docicd')]) {
          sh "docker login -u kingajith -p ${docicd}"
        }
        sh 'docker push kingajith/java-web-app'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8081:8080 --name java-web-app kingajith/java-web-app'
         
         sshagent(['DOCKER_SERVER']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.14.181 docker stop java-web-app || true'
          sh 'ssh  ubuntu@172.31.14.181 docker rm java-web-app || true'
          sh 'ssh  ubuntu@172.31.14.181 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.14.181 ${dockerRun}"
       }
       
    }
     
     
}
