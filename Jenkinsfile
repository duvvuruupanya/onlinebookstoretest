pipeline {
    agent any

    tools {
        maven 'Maven3'
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
        echo "Deploying .war to Tomcat..."
        
        sh 'scp -o StrictHostKeyChecking=no target/onlinebookstore.war deployer@192.168.11.221:/usr/share/tomcat/webapps/'
        
        sh 'ssh -o StrictHostKeyChecking=no deployer@192.168.11.221 "sudo systemctl restart tomcat"'
    }
}



    }
}
