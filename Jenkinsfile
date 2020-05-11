//node('master')

node
{
	def mavenHome = tool name: "maven3.6.3"
	
	properties([[$class: 'JiraProjectProperty'], buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '9')), pipelineTriggers([githubPush()])])
  
	stage('Checkout')
	{
	git branch: 'development', credentialsId: 'aad98cb6-fc85-4a63-8a40-118df5ad0dca', url: 'https://github.com/Xyberhaunter/maven-web-application.git'
	}
	
	stage('Build')
	{
	sh "${mavenHome}/bin/mvn clean package" 
	//bat for windows
	} 
	
	stage('SonarQubeReport')
	{
	sh "${mavenHome}/bin/mvn sonar:sonar" 
	//bat for windows
	} 
	
	stage('SonarQubeReport')
	{
	sh "${mavenHome}/bin/mvn deploy" 
	//bat for windows
	} 
	
	stage('DeployToTomcat')
	{ sshagent(['853e5ffc-4456-4fcd-8a5c-936a528935c5']) 
	 {
	sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.126.72.237://opt/apache-tomcat-9.0.34/webapps"
	 }
	}
	
	stage('EmailNotification')
	{
	emailext body: '''The lastest build was successfull.

Keep developing, keep Growing!!''', subject: 'Build Success!!', to: 'learningbott@gmail.com, devopsautomationbot@gmail.com'
	}
}
