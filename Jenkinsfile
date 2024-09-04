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
} // This is dummy text to remove