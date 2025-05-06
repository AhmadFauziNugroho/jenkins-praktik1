pipeline {
    agent any {
            image 'python:3.10'
        }
    }

    environment {
        VENV = 'venv'
    }


    stages {
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'pytest test_app.py'
            }
        }
        stage('Deploy') {
            when {
                anyOf {
                    branch 'main'
                    branch pattern: "release/.*", comparator: "REGEXP"
                }
            }
            steps {
                echo "Simulating deploy from branch ${env.BRANCH_NAME}"
            }
        }
    }

    post {
        success {
            script {
                def payload = [
                    content: " BUILD SUCCESS on branch\nURL: ${env.BUILD_URL}"
                ]

                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discord.com/api/webhooks/1369171323204407337/93tCXbegQ-YBsh_GxKJg6i7hSu3tK-ZZDqhAA7VTy610mV80qLMoaDh7VO4daYpurAgu'
                )
            }
        }
        failure {
            script {
                def payload = [
                    content: " BUILD FAILED on branch\nURL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discord.com/api/webhooks/1369171323204407337/93tCXbegQ-YBsh_GxKJg6i7hSu3tK-ZZDqhAA7VTy610mV80qLMoaDh7VO4daYpurAgu'
                )
            }
        }
    }
}
