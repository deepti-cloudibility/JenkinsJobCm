node {
    stage('Checkout Source code') {
    	echo "Checking out latest from Github.."
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'cdc8bad9-d6c7-44ad-a45b-181805d80465', url: 'https://github.com/cloudibility/channelmanager-proxy.git']]])
    }

    stage('Building code using Maven') {
    		echo "Building code using Maven"

    		mvn package docker:build
    		def pom = readMavenPom file:'pom.xml'
            print pom.version
            env.version = pom.version

        
    }

    stage('Image') {
            dir ('HotelRoom-allocation-service') {
                def app = docker.build "localhost:5000/HotelRoom-allocation-service:${env.version}"
                app.push()
    
    }

    
    stage ('Run') {
            docker.image("localhost:5000/discovery-service:${env.version}").run('-p 8761:8761 -h discovery --name discovery')
        }
 
    stage ('Final') {
            build job: 'account-service-pipeline', wait: false
        }      
}