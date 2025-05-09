def setupPython(String repoUrl, String dirName) {
    dir(dirName) {
        git url: repoUrl, branch: 'main'

        echo 'Verifying repository contents:'
        bat 'dir /b /a:-d'

        echo 'Installing python dependencies'
        bat '''
            python --version
            pip3 --version
            pip3 install -r requirements.txt
        '''
    }
}

pipeline {
    agent any

    environment {
        GREETINGS_REPO = 'https://github.com/mtararujs/python-greetings.git'
        JS_TESTS_REPO  = 'https://github.com/mtararujs/course-js-api-framework.git'
        GREETINGS_WORKDIR = 'python-greetings'
        API_TESTS_WORKDIR = 'api-tests'
        PROCESS_NAME   = 'jenkins-automation-tests' 
    }

    stages {
        stage('install-pip-deps') {
            steps {
                echo 'Installing all required dependencies...'
                script {
                    setupPython(env.GREETINGS_REPO, env.GREETINGS_WORKDIR)
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
                    bat 'pm2 delete greetings-app-dev || exit /b 0'

                    echo 'Starting DEV PM2 service on port 7001'
                    bat 'pm2 start app.py --name greetings-app-dev -- --port 7001'
                }
            }
        }

        stage('tests-on-dev') {
            steps {
                echo 'Running tests on DEV environment...'
                dir ('api-tests') {
                    echo 'Cloning api-tests repo...'
                    git url: env.JS_TESTS_REPO, branch: 'main'

                    echo 'Installing npm dependencies'
                    bat 'npm install'

                    echo 'Executing DEV tests'
                    bat 'npm run greetings greetings_dev'

                    echo 'Stopping existing DEV PM2 service'
                    bat 'pm2 delete greetings-app-dev || exit /b 0'
                }
            }
        }

        stage('deploy-to-staging') {
            steps {
                echo 'Deploying to STAGING environment...'
                dir ('python-greetings') {
                    echo 'Cloning greeting repo...'
                    git url: env.GREETINGS_REPO, branch: 'main'

                    echo 'Stopping existing STAGING PM2 service'
                    bat 'pm2 delete greetings-app-stg || exit /b 0'

                    echo 'Starting STAGING PM2 service on port 7002'
                    bat 'pm2 start app.py --name greetings-app-stg -- --port 7002'
                }
            }
        }

        stage('tests-on-staging') {
            steps {
                echo 'Running tests on STAGING environment...'
                dir ('api-tests') {
                    echo 'Cloning api-tests repo...'
                    git url: env.JS_TESTS_REPO, branch: 'main'

                    echo 'Installing npm dependencies'
                    bat 'npm install'

                    echo 'Executing STAGING tests'
                    bat 'npm run greetings greetings_stg'

                    echo 'Stopping existing STAGING PM2 service'
                    bat 'pm2 delete greetings-app-stg || exit /b 0'
                }
            }
        }

        stage('deploy-to-preprod') {
            steps {
                echo 'Deploying to PREPROD environment...'
                dir ('python-greetings') {
                    echo 'Cloning greeting repo...'
                    git url: env.GREETINGS_REPO, branch: 'main'

                    echo 'Stopping existing PREPROD PM2 service'
                    bat 'pm2 delete greetings-app-preprod || exit /b 0'

                    echo 'Starting PREPROD PM2 service on port 7003'
                    bat 'pm2 start app.py --name greetings-app-preprod -- --port 7003'
                }
            }
        }

        stage('tests-on-preprod') {
            steps {
                echo 'Running tests on PREPROD environment...'
                dir ('api-tests') {
                    echo 'Cloning api-tests repo...'
                    git url: env.JS_TESTS_REPO, branch: 'main'

                    echo 'Installing npm dependencies'
                    bat 'npm install'

                    echo 'Executing PREPROD tests'
                    bat 'npm run greetings greetings_preprod'

                    echo 'Stopping existing PREPROD PM2 service'
                    bat 'pm2 delete greetings-app-preprod || exit /b 0'
                }
            }
        }

        stage('deploy-to-prod') {
            steps {
                echo 'Deploying to PROD environment...'
                dir ('python-greetings') {
                    echo 'Cloning greeting repo...'
                    git url: env.GREETINGS_REPO, branch: 'main'

                    echo 'Stopping existing PROD PM2 service'
                    bat 'pm2 delete greetings-app-prod || exit /b 0'

                    echo 'Starting PROD PM2 service on port 7004'
                    bat 'pm2 start app.py --name greetings-app-prod -- --port 7004'
                }
            }
        }

        stage('tests-on-prod') {
            steps {
                echo 'Running tests on PROD environment...'
                dir ('api-tests') {
                    echo 'Cloning api-tests repo...'
                    git url: env.JS_TESTS_REPO, branch: 'main'

                    echo 'Installing npm dependencies'
                    bat 'npm install'

                    echo 'Executing PROD tests'
                    bat 'npm run greetings greetings_prod'

                    echo 'Stopping existing PROD PM2 service'
                    bat 'pm2 delete greetings-app-prod || exit /b 0'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished with status ${currentBuild.currentResult}'
        }
    }
}