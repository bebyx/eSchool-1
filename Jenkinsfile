#!/usr/bin/env groovy

pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Build') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn clean package -Dmaven.test.failure.ignore=true"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                    sh 'scp -o "StrictHostKeyChecking=no" target/*.jar bebyx@10.156.0.11:/home/bebyx/CI/'
                    sh 'scp -o "StrictHostKeyChecking=no" target/*.jar bebyx@10.156.0.12:/home/bebyx/CI/'
                }
            }
        }
    }
}
