pipeline {
    agent any
    environment {
        LOG_FILE = 'echo.log'
        EMAIL = 'cooperjake@deakin.edu.au'
    }


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
                echoAndLog('Running unit and integration tests...')
                // Tools: PyTest
                // Command: pytest hello_world.py
            }
            post {
                always {
                    sendEmail('Unit and integration tests')
                }
                /*success {
                    echo 'Sending status success email'
                    mail to: "cooperjake@deakin.edu.au",
                        subject: "Build Status Success Email",
                        body: "Unit and Integration Tests were successful"
                    sleep(time: 1, unit: 'SECONDS')
                }
                failure {
                    echo 'Sending status failure email'
                    mail to: "cooperjake@deakin.edu.au",
                        subject: "Build Status Failure Alert",
                        body: "Unit and Integration Tests have failed. Check Jenkins logs for more details"
                }*/
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
                echoAndLog('Performing security scan...')
                // Tool: Bandit
                // Command: bandit -r jenkins_pipeline_project/
            }
            post {
                always {
                    sendEmail('Security scan')
                }
                /*success {
                    echo 'Sending status success email'
                    mail to: "cooperjake@deakin.edu.au",
                        subject: "Build Status Success Email",
                        body: "Security Scan was successful"
                    sleep(time: 1, unit: 'SECONDS')
                }
                failure {
                    echo 'Sending status failure email'
                    mail to: "cooperjake@deakin.edu.au",
                        subject: "Build Status Failure Alert",
                        body: "Security Scan has failed. Check Jenkins logs for more details"
                }*/
                
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
                echoAndLog('Running integration tests on staging...')
                // Tool: Selenium
                // Command: pytest --selenium
            }
            post {
                always {
                    sendEmail('Integration tests on staging')
                }
                /*success {
                    echo 'Sending status success email'
                    mail to: "cooperjake@deakin.edu.au",
                         subject: "Build Status Success Email",
                        body: "Integration Tests on Staging were successful"
                    sleep(time: 1, unit: 'SECONDS')
                }
                failure {
                    echo 'Sending status failure email'
                    mail to: "cooperjake@deakin.edu.au",
                        subject: "Build Status Failure Alert",
                        body: "Integration Tests on Staging have failed. Check Jenkins logs for more details"
                }*/
            }
        }
        stage('Deploy to Production') {
            steps {
                echoAndLog('Deploying to production...')
                // Tool: AWS CLI
                // Command: aws deploy
            }
        }
    }
    post {
        always {
            sendEmail('Full pipeline')
        }
        /*success {
            echo 'Sending status success email'
            mail to: "cooperjake@deakin.edu.au",
                subject: "Build Status Success",
                body: "All building stages were successful. Application deployed to production."
            sleep(time: 1, unit: 'SECONDS')
        }
        failure {
            echo 'Sending status failure email'
            mail to: "cooperjake@deakin.edu.au",
                subject: "Build Status Failure Alert",
                body: "Something has failed. Check Jenkins logs for more details"
        }*/
    }
}

def echoAndLog(message) {
    echo message
    script {
        sh "echo '${message}' >> ${env.LOG_FILE}"
    }
}

def sendEmail(stage) {
    script {
        def buildStatus = currentBuild.result ?: 'SUCCESS'
        def subject = "Build status ${buildStatus} - ${stage}"
        def body = "${stage} ${buildStatus.toLowerCase()}.\nCheck Jenkins logs for more details."

        emailext(
            to: env.EMAIL,
            subject: subject,
            body: body,
            attachLog: true,
            attachmentsPattern: env.LOG_FILE
        )
        
        sleep(time: 1, unit: 'SECONDS')
    }
}