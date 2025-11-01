pipeline {
    agent any

    tools {
        maven 'Maven3'   // Tool name from Jenkins → Global Tool Configuration
        // Remove jdk 'jdk25' if not configured
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          userRemoteConfigs: [[url: 'https://github.com/devarshi2002/spring-petclinic.git']]])
            }
        }

        stage('Build') {
            steps {
                echo 'Building project with Maven...'
                bat 'mvn -B -DskipTests=true clean package'
            }
        }
      

        stage('Archive') {
            steps {
                echo 'Archiving artifacts...'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Deliver') {
            steps {
                echo 'Delivering build artifacts (placeholder stage)'
            }
        }
    }

    post {
        success {
            echo '✅ Build finished successfully!'
        }
        failure {
            echo '❌ Build failed! Please check logs.'
        }
    }
}
