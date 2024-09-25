pipeline {
    agent any

    tools {
        maven 'maven' // Ensure Maven is installed in Jenkins
    }

    environment {
        NEXUS_URL = "http://57.152.82.180:8081"
        NEXUS_REPOSITORY = "maven-snapshots"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Check out the code from GitHub
                    git branch: 'main',
                        url: 'https://github.com/KasthuDhiva/spring-hello-app.git',
                        credentialsId: 'Kasthu_github'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'mvn clean package' // Build the application
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests
                    sh 'mvn test'
                }
            }
        }

        
        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus-credentials', usernameVariable: 'NEXUS_USERNAME', passwordVariable: 'NEXUS_PASSWORD')]) {
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3', // Change to 'nexus2' if you're using Nexus 2.x
                        protocol: 'http',
                        nexusUrl: '57.152.82.180:8081', // Your Nexus URL
                        groupId: 'com.samplecode', // Group ID of the artifact
                        version: '0.0.1-SNAPSHOT', // Version of the artifact
                        repository: 'nestle', // The Nexus repository name (in this case, `poc`)
                        credentialsId: 'nexus-credentials',
                        artifacts: [
                            [
                                artifactId: 'spring-first-app', // Artifact ID
                                classifier: '',
                                file: 'target/spring-first-app-0.0.1-SNAPSHOT.jar', // Path to the built artifact (JAR file)
                                type: 'jar'
                            ]
                        ]
                    )
                }
            }
        }
    }

    post {
        success {
            echo 'Build, test, and deploy completed successfully!'
        }
        failure {
            echo 'Build or deploy failed!'
        }
    }
}
