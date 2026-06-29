node{

	echo " Jenkins home dir is: ${env.JENKINS_HOME}"
	echo " Jenkins node name is: ${env.NODE_NAME}"
	echo " Jenkins job name is: ${env.JOB_NAME}"
	
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
    timestamps {
        //some block
    }
    def mavenHome = tool name: 'Maven3.9.16'
	stage('CheckOutCode'){	
	git branch: 'development', credentialsId: 'baf2bd2a-ba1a-44db-959c-f4b1e1052841', url: 'https://github.com/dnakkala/Maven-Web-Application.git'
	}
	stage('Build'){
	sh "${mavenHome}/bin/mvn clean package"
	}
	stage('ExecuteSonarQubeReport'){
	 sh "${mavenHome}/bin/mvn clean sonar:sonar"
	 }
	stage('UploadArtifactIntoNexus'){
	 sh "${mavenHome}/bin/mvn clean deploy"
	 }
     
	 stage('DeployAppIntoTomcatServer'){
      sshagent(['bfb5d855-d89e-4d16-8e64-1d87b116c531']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.34.164:/opt/apache-tomcat-9.0.118/webapps/"
         }
		 }
	
}

