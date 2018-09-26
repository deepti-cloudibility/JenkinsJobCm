node {
 
    withMaven(maven:'maven') {
 
        stage('Checkout') {
            git url: 'https://github.com/piomin/sample-spring-microservices.git', credentialsId: 'github-piomin', branch: 'master'
        }
 
        stage('Build') {
            sh '/opt/apache-maven-3.5.4/bin/mvn clean install'
 
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
 
}
