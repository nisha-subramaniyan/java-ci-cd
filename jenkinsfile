pipeline {
    agent any

    tools {
        maven 'maven3.9.11'
        jdk 'JDK21'
    }

    environment {
        GIT_REPO = 'https://github.com/nisha-subramaniyan/java-ci-cd.git'
        CREDENTIALS_ID = 'github-credentials'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: "${GIT_REPO}",
                    credentialsId: "${CREDENTIALS_ID}"
            }
        }

        stage('Build with Maven') {
            steps {
                echo 'Building the Java project using Maven...'
                sh 'mvn clean package'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }

        stage('Post Build') {
            steps {
                echo 'Build completed. Notification will be sent if configured.'
            }
        }
    }

    post {
        success {
            mail to: 'nishasubramaniyan.s@gmail.com',
                 subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """Hello Nisha,

The Jenkins build for your project was successful.

Job Name: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Build URL: ${env.BUILD_URL}

Regards,
Jenkins CI/CD System"""
        }
        failure {
            mail to: 'nishasubramaniyan.s@gmail.com',
                 subject: "FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """Hello Nisha,

The Jenkins build has failed.

Job Name: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Build URL: ${env.BUILD_URL}

Please check the Jenkins console for more details."""
        }
    }
}
