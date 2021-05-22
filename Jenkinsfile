node
{
 def mavenHome = tool name: "maven3.8.1"
 properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '5', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
 stage('CheckoutCode')
 {
 git branch: 'development', credentialsId: 'b28fcc60-1bda-4b8a-b6c0-785b23bfc6f6', url: 'https://github.com/tarak-fc-apps/maven-web-application.git'
 }
 stage('Build')
 {
  sh "${mavenHome}/bin/mvn clean package"
}
stage('ExecuteSonarqubeReport')
 {
  sh "${mavenHome}/bin/mvn sonar:sonar"
}
 stage('UploadArtifactsIntoNexus')
 {
  sh "${mavenHome}/bin/mvn deploy"
}
stage('DeployApplicationIntoTomcatServer')
 {
 sshagent(['bd396e66-b639-4107-bcd4-34322b2c00ca']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.234.17.232:/opt/apache-tomcat-9.0.45/webapps/"
}
}
 stage('SendEmailNotification')
 {
 emailext body: '''builded by tarakaramireddy in devops tools 
tarak reddy
7989959774''', subject: 'build over', to: 'tarakaramireddy543@gmail.com'
}
}
