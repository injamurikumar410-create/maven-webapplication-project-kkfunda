//this is scripted-way pipeline
node
{
  // tiggers pull scm for every one minute
  properties([
    pipelineTriggers([
      pollSCM('* * * * *')
    ])
  ])
  
def mavenHome=tool name: "maven-3.9.0"
  stage('checkout')
  {
    git branch: 'dev', url: 'https://github.com/injamurikumar410-create/maven-webapplication-project-kkfunda.git'
  } 
 /* stage('compile')
  {
    sh "${mavenHome}/bin/mvn compile"
  }
  */
  stage('BUILD')
  {
     sh "${mavenHome}/bin/mvn clean package"
  }
  stage('sonar report')
  {
   sh "${mavenHome}/bin/mvn sonar:sonar"
  }
  stage('deploy to nexus')
  {
   sh "${mavenHome}/bin/mvn deploy"
  }
 stage('Deploy to Tomcat') 
{
    if (env.BRANCH_NAME == 'dev') 
    {
        withCredentials([usernamePassword(credentialsId: 'tomcat-cred-1', usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASS')]) 
        {
            sh """
            curl -v -u $TOMCAT_USER:$TOMCAT_PASS \
            --upload-file ${env.WORKSPACE}/target/maven-web-application.war \
            "http://54.224.72.95:8080/manager/text/deploy?path=/maven-web-application&update=true"
            """
        }
    } 
    else 
    {
        echo "Skipping deployment because branch is ${env.BRANCH_NAME}"
    }
}
} // node ends here
      
