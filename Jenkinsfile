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

def deployToEnv(String repoUrl, String dirName, String envName, int port) {
    dir (dirName) {
        echo 'Cloning greeting repo...'
        git url: repoUrl, branch: 'main'

        echo "Stopping existing ${envName} PM2 service"
        bat "pm2 delete greetings-app-${envName} || exit /b 0"

        echo "Starting ${envName} PM2 service on port ${port}"
        bat "pm2 start app.py --name greetings-app-${envName} -- --port ${port}"
    }
}

def runEnvTests(String repoUrl, String dirName, String envName) {
    dir (dirName) {
        echo 'Cloning api-tests repo...'
        git url: repoUrl, branch: 'main'

        echo 'Installing npm dependencies'
        bat 'npm install'

        echo "Executing ${envName} tests"
        bat "npm run greetings greetings_${envName}"

        echo "Stopping existing ${envName} PM2 service"
        bat "pm2 delete greetings-app-${envName} || exit /b 0"
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
                script {
                    deployToEnv(env.GREETINGS_REPO, env.GREETINGS_WORKDIR, 'dev', 7001)
                }
            }
        }

        stage('tests-on-dev') {
            steps {
                echo 'Running tests on DEV environment...'
                script {
                    runEnvTests(env.JS_TESTS_REPO, env.API_TESTS_WORKDIR, 'dev')
                }
            }
        }

        stage('deploy-to-staging') {
            steps {
                echo 'Deploying to STAGING environment...'
                script {
                    deployToEnv(env.GREETINGS_REPO, env.GREETINGS_WORKDIR, 'stg', 7002)
                }
            }
        }

        stage('tests-on-staging') {
            steps {
                echo 'Running tests on STAGING environment...'
                script {
                    runEnvTests(env.JS_TESTS_REPO, env.API_TESTS_WORKDIR, 'stg')
                }
            }
        }

        stage('deploy-to-preprod') {
            steps {
                echo 'Deploying to PREPROD environment...'
                script {
                    deployToEnv(env.GREETINGS_REPO, env.GREETINGS_WORKDIR, 'preprod', 7003)
                }
            }
        }

        stage('tests-on-preprod') {
            steps {
                echo 'Running tests on PREPROD environment...'
                script {
                    runEnvTests(env.JS_TESTS_REPO, env.API_TESTS_WORKDIR, 'preprod')
                }
            }
        }

        stage('deploy-to-prod') {
            steps {
                echo 'Deploying to PROD environment...'
                script {
                    deployToEnv(env.GREETINGS_REPO, env.GREETINGS_WORKDIR, 'prod', 7004)
                }
            }
        }

        stage('tests-on-prod') {
            steps {
                echo 'Running tests on PROD environment...'
                script {
                    runEnvTests(env.JS_TESTS_REPO, env.API_TESTS_WORKDIR, 'prod')
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