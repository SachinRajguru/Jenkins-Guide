// Multi-stage multi-agent pipeline
// Each stage runs in its own Docker container

pipeline {

    agent none

    stages {

        stage('Frontend') {
            agent {
                docker {
                    image 'node:20-alpine'
                }
            }
            steps {
                sh 'node --version'
            }
        }

        stage('Backend') {
            agent {
                docker {
                    image 'maven:3.8'
                }
            }
            steps {
                sh 'mvn --version'
            }
        }

        stage('Database') {
            agent {
                docker {
                    image 'mysql:8.4'
                }
            }
            steps {
                sh 'mysql --version'
            }
        }

    }

}