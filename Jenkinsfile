node{

def mavenHome = tool name: 'maven3.9.8'

echo "The job name is : ${env.JOB_NAME}"
echo "The jenkins home dir is : ${env.JENKINS_HOME}"
echo "The jenkins node name is : ${env.NODE_NAME}"
echo "The Build number is : ${env.BUILD_NUMBER}"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'RebuildSettings', autoRebuild: false, rebuildDisabled: false], [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckoutCode'){
git branch: 'development', credentialsId: '062eab6e-05b5-41c4-8910-4c55b74962a0', url: 'https://github.com/AnjanaKrishna9181/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('UploadArtifactInNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployAppIntoTomcat'){
sshagent(['3d5b618a-177e-4610-8120-048d667ad544']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.6.60:/opt/apache-tomcat-9.0.89/webapps"
}
}

}
