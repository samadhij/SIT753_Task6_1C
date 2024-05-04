pipeline {
    agent any
    stages{
        stage('Build') {
            steps {
                script {
                    sleep time: 30, unit: 'SECONDS'
                }
                echo "Building the code using Maven"
                echo "sh 'mvn clean package'"
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo "Running unit tests"
                echo "Running integration tests"
                echo "sh 'mvn test'"
            }
            post {
                success {
                    emailNotification('Unit and Integration Tests', 'Success')
                }
                failure {
                    emailNotification('Unit and Integration Tests', 'Failure')
                }
            }
        }
        stage('Code Analysis') {
            steps {
                echo "Performing code analysis using SonarQube"
                echo "sh 'sonar-scanner'"
            }
        }
        stage('Security Scan') {
            steps {
                echo "Performing security scan using OWASP Dependency-Check"
                echo "sh 'dependency-check.sh --project your_project_name --scan your_project_directory'"
            }
            post {
                success {
                    emailNotification('Security Scan', 'Success')
                }
                failure {
                    emailNotification('Security Scan', 'Failure')
                }
            }
        } 
        stage('Deploy to Staging') {
            steps {
                echo "Deploying the application to staging server using AWS CLI"
                echo "sh 'aws deploy ...'"
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging environment'
                echo "sh 'mvn integration-test'"
            }
        }
        stage('Deploy to Production') {
            steps {
                echo "Deploying the application to production server using AWS CLI"
                echo "sh 'aws deploy ...'"
                
            }
        }

    }

    post {
        success {
            echo 'Pipeline ran successfully!'
            emailext subject: 'Pipeline Status - Success',
                      body: 'The pipeline ran successfully.',
                      to: 'samadhi0727@gmail.com'
        }
        failure {
            echo 'Pipeline failed!'
            emailext subject: 'Pipeline Status - Failure',
                      body: 'The pipeline failed. Please check the logs for details.',
                      to: 'samadhi0727@gmail.com'
        }
    }
        
}
def emailNotification(stageName, status) {
    emailext subject: "Pipeline Status - $status: $stageName",
              body: "The $stageName stage ${status.toLowerCase()}. Please see attached logs for details.",
              to: 'samadhi0727@gmail.com',
              replyTo: 'samadhijayas@gmail.com',
              attachLog: true,
              attachmentsPattern: '**/*.log'
}

