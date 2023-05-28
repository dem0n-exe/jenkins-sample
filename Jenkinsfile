def skipRemainingStages = false
def statusCode = 0

pipeline {
    agent {
        node {
            //must be set in the node's setting in jenkins settings
            label 'built-in'
            customWorkspace "${JENKINS_HOME}/workspace/${JOB_NAME}/${BUILD_NUMBER}"
        }
    }
    options { disableConcurrentBuilds() }
    //https://stackoverflow.com/questions/59425340/jenkins-job-separate-workspace-by-build
    post {
        cleanup {
            deleteDir()
            dir("${workspace}@tmp") {
                deleteDir()
            }
            dir("${workspace}@script") {
                deleteDir()
            }
        }
    }
    environment {
        AWS_ACCESS_KEY = credentials('aws-access-key-katia')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key-katia')
        PATH = "/usr/bin/:/usr/local/bin:${PATH}"
        GITHUB_PAT = credentials('github-pat-katia')
        IS_PRODUCTION = "false"
        RUNNER_ENVIRONMENT = "jenkins"
        LOG_LOCATION = "terminal"
        PYTHONPATH = "${env.WORKSPACE}"
        PIPENV_VENV_IN_PROJECT = 1
    }
    parameters {
        string(name: 'PLACE_HOLDER', defaultValue: 'placeholder', description: 'placeholder')
    }
    stages {
        stage('Check System') {
            steps {
                sh 'echo $PATH'
                sh 'which python3'
                sh 'python3 --version'
                sh 'which pip3'
                sh 'pip3 --version'
                sh 'pipenv --version'
                sh 'packer --version'
                sh 'printenv'
            }
        }
        stage('Noti Test') {
            steps {
                slackSend(channel: 'test-alerts', message: 'Please choose an option:', attachments: [
    [
        fallback: 'Please choose an option:',
        actions: [
            [
                type: 'button',
                text: 'Button 1',
                url: 'https://example.com/button1'
            ],
            [
                type: 'button',
                text: 'Button 2',
                url: 'https://example.com/button2'
            ]
        ]
    ]
])

            }
        }

}