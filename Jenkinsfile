node('node1')
{
    echo "GitHub BranhName ${env.BRANCH_NAME}"
    echo "Jenkins Job Number ${env.BUILD_NUMBER}"
    echo "Jenkins Node Name ${env.NODE_NAME}"
  
    echo "Jenkins Home ${env.JENKINS_HOME}"
    echo "Jenkins URL ${env.JENKINS_URL}"
    echo "JOB Name ${env.JOB_NAME}"
 properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
 def mavenHome =tool name: "maven3.6.3"
 stage ("checkout code")
  {
     git branch: 'development', credentialsId: '2b10941f-0cfc-4ddc-bdae-296aa497be5c', url: 'https://github.com/hemaraj984/maven-web-application.git'
	 
  }
  stage("Build")
  {
  sh "${mavenHome}/bin/mvn clean package"
  }
  stage("ExecuteSonarQubeReport")
  {
  sh "${mavenHome}/bin/mvn sonar:sonar"
  }
  stage("upload artifactinto Nexus")
  {
  sh "${mavenHome}/bin/mvn deploy"
  }
   stage("deploy to Tomcat server")
   {
   sshagent(['0a073893-a905-440b-8854-681cb1e4abbc']) 
   {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.7.73.204:/opt/apache-tomcat-9.0.31/webapps/"
   }
   }
   stage("send email")
  {
  emailext body: '''Build is over....

   Regards
    hema
    9949602130''', subject: 'Build is over!!', to: 'hemaraj7677@gmail.com'
  }


}
