node {
    try {
        notifyBuild('STARTED')

        stage('Prepare code') {
            echo 'do checkout stuff'
        }

        stage('Testing') {
            echo 'Testing'
            echo 'Testing - publish coverage results'
        }

        stage('Staging') {
            echo 'Deploy Stage'
        }

        stage('Deploy') {
            echo 'Deploy - Backend'
            echo 'Deploy - Frontend'
        }

  } catch (e) {
    // If there was an exception thrown, the build failed
    currentBuild.result = "FAILED"
    throw e
  } finally {
    // Success or failure, always send notifications
    notifyBuild(currentBuild.result)
  }
}

def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}


node{
  echo "jod Name is : ${env.JOB_NAME}"
  echo "node name is: ${env.NODE_NAME}"
 properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: '']])
  def mavenHome=tool name:'maven 3.8.5'
   //Get the code from Github
    stage('checkoutcode'){
        git branch: 'development', credentialsId: '7d97adbd-b318-42bf-826c-1519725da18e', url: 'https://github.com/ravi-devops-s/maven-web-application.git'
         }
 //Do the Build
    stage('Build'){
        sh "${mavenHome}/bin/mvn clean package"
         }
//execute Sonarqube Report
stage('sonarqube report'){
    sh "${mavenHome}/bin/mvn sonar:sonar"
}
 //upload to artifactory Repository
 stage('artifactory Repository'){
     sh "${mavenHome}/bin/mvn deploy"
 }
/*// Deploy application in Tomcat server
stage('Tomcat'){
    sshagent(['ce0e1265-5025-45d4-8cef-af9782b3aaee']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.34.65:/apache-tomcat-9.0.62/webapps/"
    }
} */
}
