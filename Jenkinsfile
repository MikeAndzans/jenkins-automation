pipeline {
    agent any

    environment {
        GREETINGS_REPO = 'https://github.com/mtararujs/python-greetings.git'
        JS_TESTS_REPO  = 'https://github.com/mtararujs/course-js-api-framework.git'
        PROCESS_NAME   = 'jenkins-automation-tests' 
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
                        pip3 --version
                        pip3 install -r requirements.txt
                    '''
                }
            }
        }

        stage('deploy-to-dev') {
            steps {
                echo 'Deploying to DEV environment...'
                dir ('python-greetings') {
                    echo 'Cloning greeting repo...'
                    git url: env.GREETINGS_REPO, branch: 'main'

                    echo 'Stopping existing DEV PM2 service'
                    pwsh 'pm2 delete greetings-app-dev; if ($LASTEXITCODE -ne 0) { exit 0 }'

                    echo 'Starting DEV PM2 service on port 7001'
                    pwsh 'pm2 start app.py --name greetings-app-dev -- --port 7001'
                }
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
                dir ('python-greetings') {
                    echo 'Cloning greeting repo...'
                    git url: env.GREETINGS_REPO, branch: 'main'

                    echo 'Stopping existing STAGING PM2 service'
                    pwsh 'pm2 delete greetings-app-stg; if ($LASTEXITCODE -ne 0) { exit 0 }'

                    echo 'Starting STAGING PM2 service on port 7001'
                    pwsh 'pm2 start app.py --name greetings-app-stg -- --port 7001'
                }
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
                dir ('python-greetings') {
                    echo 'Cloning greeting repo...'
                    git url: env.GREETINGS_REPO, branch: 'main'

                    echo 'Stopping existing PREPROD PM2 service'
                    pwsh 'pm2 delete greetings-app-preprod; if ($LASTEXITCODE -ne 0) { exit 0 }'

                    echo 'Starting PREPROD PM2 service on port 7001'
                    pwsh 'pm2 start app.py --name greetings-app-preprod -- --port 7001'
                }
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
                dir ('python-greetings') {
                    echo 'Cloning greeting repo...'
                    git url: env.GREETINGS_REPO, branch: 'main'

                    echo 'Stopping existing PROD PM2 service'
                    pwsh 'pm2 delete greetings-app-prod; if ($LASTEXITCODE -ne 0) { exit 0 }'

                    echo 'Starting PROD PM2 service on port 7001'
                    pwsh 'pm2 start app.py --name greetings-app-prod -- --port 7001'
                }
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