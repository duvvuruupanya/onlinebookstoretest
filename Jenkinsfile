pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'master', url: 'https://github.com/duvvuruupanya/onlinebookstoretest.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                  scp -o StrictHostKeyChecking=no target/onlinebookstore.war deployer@192.168.11.221:/usr/share/tomcat/webapps/
                  ssh -o StrictHostKeyChecking=no deployer@192.168.11.221 "sudo systemctl restart tomcat"
                '''
            }
        }
    }
}
