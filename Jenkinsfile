pipeline \{
 agent any
    stages \{
    stage('Checkout Source code') \{
        steps \{    
    	echo "Checking out latest from Github....."
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'cdc8bad9-d6c7-44ad-a45b-181805d80465', url: 'https://github.com/cloudibility/channelmanager-discovery.git']]])
     }
    }
    stage('Building maven') \{
        steps \{
    		echo "Building code using Maven..."
    sh '/opt/apache-maven-3.5.4/bin/mvn package docker:build'
    sh 'pwd'
    		def pom = readMavenPom file:'pom.xml'
            print pom.version
            env.version = pom.version
       }
    }
    stage('Upload') \{
        steps \{
    nexusArtifactUploader artifacts: [[artifactId: 'channelmanager-discovery', classifier: 'debug', file: 'channelmanager-discovery-0.0.1-SNAPSHOT', type: 'jar']], credentialsId: 'nexusAdmin', groupId: 'com.applicity.channelmanager', nexusUrl: '34.238.84.40:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'jenkins-artifacts', version: '0.0.1-SNAPSHOT'

             }
           }
        }
    }
