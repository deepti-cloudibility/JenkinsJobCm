node {
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
    
     stage('Image') {
            dir ('discovery-service') {
                def app = docker.build "localhost:5000/discovery-service:${env.version}"
                app.push()
            }
        }
 
        stage ('Run') {
            docker.image("localhost:5000/discovery-service:${env.version}").run('-p 8761:8761 -h discovery --name discovery')
        }
 
        stage ('Final') {
            build job: 'account-service-pipeline', wait: false
        }      
 
    }
 
