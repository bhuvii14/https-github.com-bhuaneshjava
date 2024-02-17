node{
    
   def buildNumber = BUILD_NUMBER
   
    stage("Git CheckOut"){
        git url: 'https://github.com/bhuvii14/https-github.com-bhuaneshjava.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
    } 
    stage("Build Dokcer Image") {
      sh "docker build -t bhuvaa14/javawebapp:${buildNumber} ."
    }
    stage("Docker login and Push"){
      withCredentials([string(credentialsId: 'docker1', variable: 'Docker1')]) {
      sh "docker login -u bhuvaa14 -p ${Docker1} " 
           }
      sh "docker push bhuvaa14/javawebapp:${buildNumber}"       
    
}   
    stage("Deploy to dockercontinor in docker deployer"){
      sshagent(['docker_ssh_password']) {
      sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.4.199 docker rm -f cloudcandy || true"
      sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.4.199 docker run -d -p 9000:8080 --name cloudcandy bhuvaa14/javawebapp:${buildNumber}"           
    }  
}

}
