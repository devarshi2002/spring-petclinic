pipeline {
  agent any

  tools {
    maven 'Maven3'    // must match name in Global Tool Configuration
    jdk 'jdk11'       // optional if you set a JDK tool name
  }

  environment {
    // optionally set ENV vars, e.g. MAVEN_OPTS
    MAVEN_OPTS = '-Xmx1024m'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        // use sh on Linux/macOS agents
        sh 'mvn -B -DskipTests=true clean package'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn -B test'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }

    stage('Archive') {
      steps {
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      }
    }

    stage('Deliver') {
      steps {
        echo 'Deliver: push to Nexus/Artifactory (configure credentials and steps)'
      }
    }
  }

  post {
    always {
      echo "Build finished: ${currentBuild.currentResult}"
    }
    success {
      echo 'Success! notify team or trigger downstream jobs'
    }
    failure {
      echo 'Failed! collect logs and inform'
    }
  }
}
