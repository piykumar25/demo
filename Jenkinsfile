pipeline {
  agent any

  triggers {
    // We'll start with polling; we’ll switch to webhooks in the next step
    pollSCM('H/2 * * * *')   // check for changes roughly every 2 minutes
  }

  options {
    timestamps()
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build') {
      steps {
        sh './mvnw -v'
        sh './mvnw -B -DskipTests clean package'
      }
    }

    stage('Test') {
      steps {
        sh './mvnw -B test'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }

    stage('Archive artifact') {
      steps {
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      }
    }
  }

  post {
    success {
      echo "✅ Build ${env.BUILD_NUMBER} OK"
    }
    failure {
      echo "❌ Build ${env.BUILD_NUMBER} failed"
    }
  }
}




