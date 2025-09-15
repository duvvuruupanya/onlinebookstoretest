pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/duvvuruupanya/onlinebookstoretest.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sshagent(['deploy-ssh']) {
                    sh '''
                        scp -o StrictHostKeyChecking=no target/*.war deployer@192.168.11.221:/home/deployer/
                        ssh deployer@192.168.11.221 '
                            sudo mv /home/deployer/*.war /usr/share/tomcat/webapps/ &&
                            sudo systemctl restart tomcat
                        '
                    '''
                }
            }
        }
    }
}
