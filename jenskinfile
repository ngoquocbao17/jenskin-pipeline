pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Use Maven to build the code
                sh 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                // Use JUnit and Selenium to run unit and integration tests
                sh 'mvn test'
            }
        }

        stage('Code Analysis') {
            steps {
                // Use SonarQube to analyze the code
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Security Scan') {
            steps {
                // Use OWASP ZAP to scan the code for vulnerabilities
                sh 'zap-baseline.py -t http://localhost:8080 -r security-report.html'
                archiveArtifacts 'security-report.html'
            }
        }

        stage('Deploy to Staging') {
            steps {
                // Use SSH to deploy the application to a staging server
                sshagent(['my-ssh-credentials']) {
                    sh 'ssh user@staging-server "cd /path/to/application && ./deploy.sh"'
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                // Use JUnit and Selenium to run integration tests on the staging environment
                sh 'mvn test'
            }
        }

        stage('Deploy to Production') {
            steps {
                // Use SSH to deploy the application to a production server
                sshagent(['my-ssh-credentials']) {
                    sh 'ssh user@production-server "cd /path/to/application && ./deploy.sh"'
                }
            }
        }
    }

    post {
        always {
            // Send notification email with logs as attachment
            emailext attachLog: true, body: 'Pipeline finished', subject: 'Pipeline status: ${currentBuild.currentResult}', to: 'email@example.com'
        }
    }
}
