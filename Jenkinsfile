pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                echo 'Retreive code from Github'
                // Command: checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building the project...'
                // Tool: Poetry
                // Command: poetry build
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                // Tools: PyTest
                // Command: pytest hello_world.py
            }
            post {
                success {
                    echo 'Sending status success email'
                    mail to: "cooperjake@deakin.edu.au",
                        subject: "Build Status Success Email",
                        body: "Unit and Integration Tests were successful"
                    sleep(time: 1, units: 'SECONDS')
                }
                failure {
                    echo 'Sending status failure email'
                    mail to: "cooperjake@deakin.edu.au",
                        subject: "Build Status Failure Alert",
                        body: "Unit and Integration Tests have failed. Check Jenkins logs for more details"
                }
            }
        }
        stage('Code Analysis') {
            steps {
                echo 'Running code analysis...'
                // Tool: pylint
                // Command: pylint hello_world.py
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
                // Tool: Bandit
                // Command: bandit -r jenkins_pipeline_project/
            }
            post {
                success {
                    echo 'Sending status success email'
                    mail to: "cooperjake@deakin.edu.au",
                        subject: "Build Status Success Email",
                        body: "Security Scan was successful"
                    sleep(time: 1, units: 'SECONDS')
                }
                failure {
                    echo 'Sending status failure email'
                    mail to: "cooperjake@deakin.edu.au",
                        subject: "Build Status Failure Alert",
                        body: "Security Scan has failed. Check Jenkins logs for more details"
                }
                
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging environment...'
                // Tool: AWS CLI
                // Command: aws deploy
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                // Tool: Selenium
                // Command: pytest --selenium
            }
            post {
                success {
                    echo 'Sending status success email'
                    mail to: "cooperjake@deakin.edu.au",
                         subject: "Build Status Success Email",
                        body: "Integration Tests on Staging were successful"
                    sleep(time: 1, units: 'SECONDS')
                }
                failure {
                    echo 'Sending status failure email'
                    mail to: "cooperjake@deakin.edu.au",
                        subject: "Build Status Failure Alert",
                        body: "Integration Tests on Staging have failed. Check Jenkins logs for more details"
                }
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production...'
                // Tool: AWS CLI
                // Command: aws deploy
            }
        }
    }
    post {
        success {
            echo 'Sending status success email'
            mail to: "cooperjake@deakin.edu.au",
                subject: "Build Status Success",
                body: "All building stages were successful. Application deployed to production."
            sleep(time: 1, units: 'SECONDS')
        }
        failure {
            echo 'Sending status failure email'
            mail to: "cooperjake@deakin.edu.au",
                subject: "Build Status Failure Alert",
                body: "Integration Tests on Staging have failed. Check Jenkins logs for more details"
        }        
    }
}