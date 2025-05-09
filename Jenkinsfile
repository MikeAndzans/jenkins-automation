pipeline {
    agent any

    environment {
        GREETINGS_REPO = 'https://github.com/mtararujs/python-greetings.git'
        JS_TESTS_REPO   = 'https://github.com/mtararujs/course-js-api-framework.git'
        PROCESS_NAME     = 'jenkins-automation-tests'
    }

    stages {
        stage('install-pip-deps') {
            steps {
                echo 'Installing all required dependencies...'
                dir('python-greetings') {
                    git url: env.GREETINGS_REPO, branch: 'main'

                    echo 'Verifying repository contents:'
                    pwsh 'Get-ChildItem -Force'
                    
                    echo 'Installing python dependencies'
                    pwsh '''
                        python --version
                        python -m pip install --upgrade pip
                        python -m pip install -r requirements.txt
                    '''
                }
            }
        }

        stage('deploy-to-dev') {
            steps {
                echo 'Deploying to DEV environment...'

            }
        }

        stage('tests-on-dev') {
            steps {
                echo 'Running tests on DEV environment...'

            }
        }

        stage('deploy-to-staging') {
            steps {
                echo 'Deploying to STAGING environment...'

            }
        }

        stage('tests-on-staging') {
            steps {
                echo 'Running tests on STAGING environment...'

            }
        }

        stage('deploy-to-preprod') {
            steps {
                echo 'Deploying to PREPROD environment...'

            }
        }

        stage('tests-on-preprod') {
            steps {
                echo 'Running tests on PREPROD environment...'

            }
        }

        stage('deploy-to-prod') {
            steps {
                echo 'Deploying to PROD environment...'

            }
        }

        stage('tests-on-prod') {
            steps {
                echo 'Running tests on PROD environment...'

            }
        }
    }

    post {
        always {
            echo 'Pipeline finished with status ${currentBuild.currentResult}'
        }
    }
}