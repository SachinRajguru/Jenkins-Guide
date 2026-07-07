
// Multi-stage pipeline demonstrating Docker-based agents
// Each stage runs inside its own isolated container

pipeline {

    // No global agent because each stage defines its own Docker agent
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
                    image 'maven:3.9.15-eclipse-temurin-17'
                }
            }

            steps {
                sh 'mvn -v'
            }
        }

        stage('Database') {
            agent {
                docker {
                    image 'mysql:latest'
                }
            }

            steps {
                sh 'mysql --version'
            }
        }
    }
}