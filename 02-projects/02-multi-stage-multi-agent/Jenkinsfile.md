// Multi-stage multi-agent pipeline
// Each stage runs in its own Docker container

pipeline {

    // No global agent
    // Each stage uses its own agent
    agent none

    stages {

        stage('Frontend') {

            // Frontend runs inside Node 20 container
            agent {
                docker {
                    image 'node:20-alpine'
                }
            }

            steps {

                // Verify Node is available
                sh 'node --version'

            }
        }

        stage('Backend') {

            // Backend runs inside Maven container
            agent {
                docker {
                    image 'maven:3.9.15-eclipse-temurin-17'
                }
            }

            steps {

                // Verify Maven is available
                sh 'mvn --version'

            }
        }

        stage('Database') {

            // Database runs inside MySQL container
            agent {
                docker {
                    image 'mysql:8.4'
                }
            }

            steps {

                // Verify MySQL is available
                sh 'mysql --version'

            }
        }

    }

}