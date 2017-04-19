node {
   def mvnHome
   def version
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/aana1530/addressbook.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'M2_HOME'
      version='2.3.5'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'

   }   
   stage('SonarQube analysis') {
    withSonarQubeEnv('SONAR_HOME') {
      // requires SonarQube Scanner for Maven 3.2+
       sh "'${mvnHome}/bin/mvn' org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'"
    }
  }
   stage('Publish') {
     nexusPublisher nexusInstanceId:'NEXUS1', nexusRepositoryId: 'release', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'addressbook_main/target/addressbook.war']], mavenCoordinate: [artifactId: 'addressbook_main', groupId: 'com.edurekademo.tutorial', packaging: 'war', version: '2.3.0']]]
   }
}
