pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/duvvuruupanya/onlinebookstoretest.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                    scp target/*.war deployer@192.168.11.221:/usr/share/tomcat/webapps/onlinebookstore.war
                '''
            }
        }

        stage('Restart Tomcat') {
            steps {
                sh 'ssh deployer@192.168.11.221 "sudo systemctl restart tomcat"'
            }
        }
    }
}
