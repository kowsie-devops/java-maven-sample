pipeline {
    agent any

    tools {
        jdk 'Java21'           // must match the JDK name in Global Tool Configuration
        maven 'Maven3.8.7'          // must match the Maven name in Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📥 Pulling code from GitHub...'
                git branch: 'main', credentialsId: 'github-token', url: 'https://github.com/kowsie-devops/java-maven-sample.git'
            }
        }

        stage('Build') {
            steps {
                echo '🏗️ Building with Maven...'
                sh 'mvn clean package'
            }
        }

        stage('Archive Artifacts') {
            steps {
                echo '📦 Archiving build artifacts...'
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Build Successful!'
            emailext (
                subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                <p>Hi Team,</p>
                <p>The Jenkins build <b>${env.JOB_NAME}</b> #${env.BUILD_NUMBER} was <b>successful</b>.</p>
                <p>Check details here: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                <br>
                <p>Regards,<br>Jenkins CI</p>
                """,
                mimeType: 'text/html',
                to: 'kowsi629@gmail.com'
            )
        }

        failure {
            echo '❌ Build Failed!'
            emailext (
                subject: "❌ FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                <p>Hi Team,</p>
                <p>The Jenkins build <b>${env.JOB_NAME}</b> #${env.BUILD_NUMBER} has <b>failed</b>.</p>
                <p>Please review the console output here: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                <br>
                <p>Regards,<br>Jenkins CI</p>
                """,
                mimeType: 'text/html',
                to: 'kowsi629@gmail.com'
            )
        }

        unstable {
            echo '⚠️ Build is unstable.'
            emailext (
                subject: "⚠️ UNSTABLE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                <p>Hi Team,</p>
                <p>The Jenkins build <b>${env.JOB_NAME}</b> #${env.BUILD_NUMBER} is <b>unstable</b>.</p>
                <p>Check details here: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                <br>
                <p>Regards,<br>Jenkins CI</p>
                """,
                mimeType: 'text/html',
                to: 'kowsi629@gmail.com'
            )
        }
    }
}

