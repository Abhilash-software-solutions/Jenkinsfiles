

node {

def mavenHome= tool name: 'maven3.6.3'
    
    //echo "GitHub BranhName ${env.BRANCH_NAME}"
  echo "Jenkins Job Number ${env.BUILD_NUMBER}"
  echo "Jenkins Node Name ${env.NODE_NAME}"
  
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"

stage('checkoutCode')
{
git branch: 'development', credentialsId: '1a511419-42a4-435c-a623-9253b29a7a08', url: 'https://github.com/Abhilash-software-solutions/maven-web-application.git'

}

stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}
stage('SonarQubeReportExecution')
{
sh "${mavenHome}/bin/mvn sonar:sonar"

}
stage('UploadArtifactsInNexus')
{
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployAppIntoTomcat')
{
sshagent(['9122cb14-0ca0-4b1e-a655-750f18fec8c7']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.234.217.202:/opt/apache-tomcat-9.0.34/webapps/"

}
}stage('EmailNotification')
{
emailext body: '''Build success 

Regards,
Abhilash''', subject: 'Build is over...', to: 'aravindramayampet@gmail.com,ramayampet.a@gmail.com'
}
}
