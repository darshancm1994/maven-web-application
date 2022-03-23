node{
def mavenHome = tool name: "maven3.8.4" 
 
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')), [$class: 'RebuildSettings', autoRebuild: false, rebuildDisabled: false], [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

stage('checkoutcode'){
git branch: 'development', credentialsId: '6d4f4283-2146-484c-9b20-e800867a1019', url: 'https://github.com/darshancm1994/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactsIntoNexus'){ 
sh "${mavenHome}/bin/mvn deploy"                
}


stage('DeployApplicationIntoTomcatServer'){
sshagent(['98ca988a-ac86-4115-bd72-f822ffb85bdf']) {
 sh "scp -o  StrictHostKeyChecking=no target/maven-web-application.war  ec2-user@3.109.2.186:/opt/apache-tomcat-9.0.58/webapps"

}

stage ('SendEmailNotification')}
emailext body: '''Build over

Regards
Darshan cm''', subject: 'Build over', to: 'darshancm143@gmail.com'
}

