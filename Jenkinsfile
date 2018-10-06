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
          }
 
  stage('Upload artifacts to nexus') {
      nexusArtifactUploader artifacts: [[artifactId: 'channelmanager-discovery', classifier: 'debug', file: 'target/docker/channelmanager-discovery-0.0.1-SNAPSHOT.jar', type: 'jar']], credentialsId: 'nexusAdmin', groupId: 'com.applicity.channelmanager', nexusUrl: '34.238.84.40:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'jenkins-artifacts', version: '$version'
                  }
 stage('upload docker images to nexus'){
       withCredentials ([string (credentialsId: 'nexusAdmin')]) {
        withEnv (["NEXUS_URL=http://34.238.84.40:8081/"]) {
          dir ('jenky-docker') {
            sh "pwd"
            dir ('docker/push') {
              sh "docker tag channelmanager-discovery:latest 34.238.84.40:8085/jenky-docker/channelmanager-discovery:latest"
              sh "docker tag channelmanager-discovery:latest 34.238.84.40:8085/jenky-docker/channelmanager-discovery:latest"
            }
          }
        }
      }
      
 stage('Building and running') {
          step([$class: 'DockerComposeBuilder', dockerComposeFile: '/opt/docker-compose.yml', option: [$class: 'StartService', scale: 1, service: 'discovery'], useCustomDockerComposeFile: true])
           }
 
    }
