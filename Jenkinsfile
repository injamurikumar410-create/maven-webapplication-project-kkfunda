

pipeline {
    agent any
tools {
maven "maven-3.9.0"
}
stages {
        stage('checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/injamurikumar410-create/maven-webapplication-project-kkfunda.git'
            }
        }
     stage('build'){
        steps{
       sh "mvn clean package"
      }
    }

   stage('sonar-qube'){
    steps{
   sh "mvn sonar:sonar"
  }
}
stage('nexus'){
steps{
sh "mvn deploy"
}
}
stage('deploy to tomcat') {
steps {
   sh """

      curl -u kk:password \
--upload-file /var/lib/jenkins/workspace/jio-dev-declarative/target/maven-web-application.war \
"http://3.89.134.101:8080/manager/text/deploy?path=/maven-web-application&update=true"
          
        """
     }
  }

    } //stages ending

} //pipeline ending  

