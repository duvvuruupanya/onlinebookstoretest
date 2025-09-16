pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                // ensures no old artifacts remain
                deleteDir()
            }
        }

        stage('Checkout Code') {
            steps {
                // always pull latest from GitHub
                git branch: 'master', url: 'https://github.com/duvvuruupanya/onlinebookstoretest.git'
            }
        }

        stage('Build WAR') {
            steps {
                // force fresh build and dependencies update
                sh 'mvn clean package -U'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                // copy the latest .war to Tomcat webapps
                sh '''
                    scp target/*.war deployer@192.168.11.221:/usr/share/tomcat/webapps/onlinebookstore.war
                '''
            }
        }

        stage('Restart Tomcat') {
            steps {
                // restart Tomcat so it picks the new WAR
                sh 'ssh deployer@192.168.11.221 "sudo systemctl restart tomcat"'
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful with the latest WAR!"
        }
        failure {
            echo "❌ Deployment failed. Check logs."
        }
    }
}
