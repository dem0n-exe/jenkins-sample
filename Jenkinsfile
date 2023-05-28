def skipRemainingStages = false
def statusCode = 0

pipeline {
    agent any

    stages {
        stage('Check System') {
            steps {
                sh 'echo $PATH'
                sh 'which python3'
                sh 'python3 --version'
                sh 'which pip3'
                sh 'pip3 --version'
                sh 'pipenv --version'
                sh 'printenv'
            }
        }
        stage('Create') {
            when {
                expression {
                    !skipRemainingStages
                }
            }
            steps {
                script {
                    statusCode = sh(script: 'pipenv run python -u main.py', returnStatus: true)
                    if (statusCode != 0) {
                        skipRemainingStages = true
                    }
                }
            }
        }
        stage('Notification') {
            steps {
                slackSend(channel: 'your_channel_id', message: 'Please choose an option:', attachments: [
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
}