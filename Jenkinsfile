pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    stages {
        stage('Checkout from GitHub') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/duvvuruupanya/onlinebookstoretest.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn -B clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sshagent(['tomcat-ssh']) {
                    sh '''
                        scp -o StrictHostKeyChecking=no target/onlinebookstore.war deployer@192.168.11.221:/usr/share/tomcat/webapps/
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful!"
        }
        failure {
            echo "❌ Deployment failed. Check Jenkins console log."
        }
    }
}
