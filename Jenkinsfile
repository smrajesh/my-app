node{
   stage('SCM Checkout'){
     git 'https://github.com/smrajesh/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   stage('Build Docker Imager'){
   sh 'docker build -t rajesh3377/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u rajesh3377 -p ${dockerPassword}"
    }
   sh 'docker push rajesh3377/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 3.109.217.180:8083"
   sh "docker tag rajesh3377/myweb:0.0.2 3.109.217.180:8083/rajesh:1.0.0"
   sh 'docker push 3.109.217.180:8083/rajesh:1.0.0'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm  tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   }
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest1 rajesh3377/myweb:0.0.2' 
   }
}
}
