pipeline {
  agent any

  tools {
    maven 'Maven3'
  }

  environment {
    TOMCAT_HOST = "192.168.56.102"   // 🔹 Replace with Tomcat VM IP
    APP_NAME    = "onlinebookstore"  // 🔹 WAR name without .war
  }

  stages {
    stage('Checkout from GitHub') {
      steps {
        checkout scm
      }
    }

    stage('Build with Maven') {
      steps {
        sh 'mvn -B clean package'
        archiveArtifacts artifacts: 'target/*.war', fingerprint: true
      }
    }

    stage('Deploy to Tomcat') {
      steps {
        sshagent (credentials: ['tomcat-ssh']) {
          sh """
            echo "➡️ Deploying WAR to Tomcat..."
            scp -o StrictHostKeyChecking=no target/*.war deployer@${TOMCAT_HOST}:/opt/tomcat/webapps/${APP_NAME}.war
          """
        }
      }
    }
  }

  post {
    success {
      echo "✅ Application deployed successfully at http://${TOMCAT_HOST}:8080/${APP_NAME}/"
    }
    failure {
      echo "❌ Deployment failed. Check Jenkins console log."
    }
  }
}
