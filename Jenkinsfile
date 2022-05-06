pipeline {
    agent any

    options {
    buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '20', numToKeepStr: '5')
    }

    stages {
        stage('Git-Checkout') {
            steps {
               git branch: 'main', credentialsId: 'Git-hub', url: 'https://github.com/anandgdk/maven-jas'
            }
        }
        stage('maven-Build') {
            steps {
               sh 'mvn clean package'
               
            }
        }
        
        stage('tomcat-dev') {
            steps {
               sshagent(['ec2-user']) {
             // some block
             sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.2.64:/opt/tomcat8/webapps/app.war`
             //stop tomcat
             sh "ssh ec2-user@172.31.2.64 /opt/tomcat8/bin/shutdown.sh"
             //start tomcat
             sh "ssh ec2-user@172.31.2.64 /opt/tomcat8/bin/startup.sh"
                    }
               
            }
        }
        
    }
}
