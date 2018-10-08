node {
 env.version = '0.0.1-SNAPSHOT'
 stage('Checkout Source code') { 
    	 echo "Checking out latest from Github....."
      checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'cdc8bad9-d6c7-44ad-a45b-181805d80465', url: 'https://github.com/cloudibility/channelmanager-discovery.git']]])
         }
 
  stage('Building maven') {
    		echo "Building code using Maven..."
      sh '/opt/apache-maven-3.5.4/bin/mvn package docker:build'
  
    		      def pom = readMavenPom file:'pom.xml'
            print pom.version
            env.version = pom.version
            sh "pwd"
           
            echo "printing environment variables"
              sh '''echo "BUILD_NUMBER" :: $BUILD_NUMBER
                 echo "BUILD_ID" :: $BUILD_ID
                 echo "BUILD_DISPLAY_NAME" :: $BUILD_DISPLAY_NAME
                 echo "JOB_NAME" :: $JOB_NAME
                 echo "JOB_BASE_NAME" :: $JOB_BASE_NAME
                 echo "BUILD_TAG" :: $BUILD_TAG'''
          }
 
 stage('Publish') {
def pom = readMavenPom file: 'pom.xml'
  withCredentials([usernamePassword(credentialsId: 'nexusAdmin', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')])
  nexusRepositoryId: "jenkins-artifacts", \
  packages: [[$class: 'MavenPackage', \
  mavenAssetList: [[classifier: '', extension: '', \
  filePath: "target/${pom.artifactId}-${pom.version}.${pom.packaging}"]], \
  mavenCoordinate: [artifactId: "${pom.artifactId}", \
  groupId: "${pom.groupId}", \
  packaging: "${pom.packaging}", \
  version: "${pom.version}"]]]
  }
 
 stage('upload docker images to nexus'){
       withCredentials([usernamePassword(credentialsId: 'nexusAdmin', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
        withEnv (["NEXUS_DOCKERURL=34.238.84.40:8085"]) {
          dir ('jenky-docker') {
            sh "pwd"
             sh 'echo uname=$USERNAME pwd=$PASSWORD'
              sh "docker login $NEXUS_DOCKERURL -u $USERNAME -p $PASSWORD"
              sh "docker tag channelmanager-discovery:latest $NEXUS_DOCKERURL/jenky-docker/channelmanager-discovery:$version"
              sh "docker push $NEXUS_DOCKERURL/jenky-docker/channelmanager-discovery:$version"
             

            }
          }
        }
      }
 stage('Building and running') {
          step([$class: 'DockerComposeBuilder', dockerComposeFile: '/opt/docker-compose.yml', option: [$class: 'StartService', scale: 1, service: 'discovery'], useCustomDockerComposeFile: true])
           }
 
    }
