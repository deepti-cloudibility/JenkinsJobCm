node{
    stage('Checkout Source code') {
    	echo "Checking out latest from Github.."
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'cdc8bad9-d6c7-44ad-a45b-181805d80465', url: 'https://github.com/cloudibility/channelmanager-discovery.git']]])
    }

    stage('Building maven') {
    		echo "Building code using Maven"
    sh '/opt/apache-maven-3.5.4/bin/mvn package docker:build'
 
    		def pom = readMavenPom file:'pom.xml'
            print pom.version
            env.version = pom.version
       }
 
    stage('Publish') {
def pom = readMavenPom file: 'pom.xml'
  nexusPublisher nexusInstanceId: 'nexus3', \
  nexusRepositoryId: 'jenkins-artifacts', \
  packages: [[$class: 'MavenPackage', \
  mavenAssetList: [[classifier: '', extension: '', \
  filePath: "target/${pom.artifactId}-${pom.version}.${pom.packaging}"]], \
  mavenCoordinate: [artifactId: "${pom.artifactId}", \
  groupId: "${pom.groupId}", \
  packaging: "${pom.packaging}", \
  version: "${pom.version}"]]]
  }
}

