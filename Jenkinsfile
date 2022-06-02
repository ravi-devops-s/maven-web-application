node{
  echo "jod Name is : ${env.JOB_NAME}"
  echo "node name is: ${env.NODE_NAME}"
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

}
