node {
   def mvnHome
   stage('Preparation') { // for display purposes
      copyArtifacts filter: 'index.jsp', fingerprintArtifacts: true, projectName: 'webdev/indexpage', selector: lastSuccessful(), target: 'src/main/webapp'
      // Get some code from a GitHub repository
      git 'https://github.com/linuxacademy/content-jenkinscert.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'M3'
   }
   stage('Build') {
      // Run the maven build
      withEnv(["MVN_HOME=$mvnHome"]) {
         if (isUnix()) {
            sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
         } else {
            bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
         }
      }
   }
   stage('tarball'){
       sh "tar -czf tomcatbuild.tar.gz src/main/webapp/* target/*"
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'tomcatbuild.tar.gz'
      fingerprint 'tomcatbuild.tar.gz'
   }
}
