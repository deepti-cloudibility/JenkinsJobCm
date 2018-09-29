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
 nexusArtifactUploader artifacts: [[artifactId: 'channelmanager-discovery', classifier: 'debug', file: '/var/lib/jenkins/workspace/rdttdcr_master-PTDWPPD6RLNY2OMGI322VIHZTVQC5AQWC2YOIV2FOJDH4XOBPNXA/target/docker//channelmanager-discovery-0.0.1-SNAPSHOT.jar', type: 'jar']], credentialsId: 'nexusAdmin1', groupId: 'com.applicity.channelmanager', nexusUrl: '34.238.84.40:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'jenkins-artifacts', version: '0.0.1-SNAPSHOT'


    }
 
}
