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
 
  stage('Upload') {
      nexusArtifactUploader artifacts: [[artifactId: 'channelmanager-discovery', classifier: 'debug', file: 'target/docker/channelmanager-discovery-0.0.1-SNAPSHOT.jar', type: 'jar']], credentialsId: 'nexusAdmin', groupId: 'com.applicity.channelmanager', nexusUrl: '34.238.84.40:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'jenkins-artifacts', version: '$version'
                  }
      
 stage('Building and running') {
           step([$class: 'DockerComposeBuilder', dockerComposeFile: 'docker-compose.yml', option: [$class: 'ExecuteCommandInsideContainer', command: 'docker-compose up -d', index: 1, privilegedMode: false, service: 'channelmanager-discovery', workDir: '/var/lib/jenkins/workspace/test01_master-YC5XRICYOG2NHN6C2YH7YS5MDNHMD3ITX4MII7VNK2M4KKVI4LJQ'], useCustomDockerComposeFile: false])

     
           }
      
 
    }
