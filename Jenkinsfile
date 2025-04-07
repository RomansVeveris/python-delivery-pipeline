def installPythonDeps() {
    bat '''
    echo "Cloning python-greetings repo..."
    if exist python-greetings rmdir /s /q python-greetings
    git clone https://github.com/mtararujs/python-greetings
    cd python-greetings
    echo "Listing repo files..."
    ls -la
    echo "Installing Python dependencies..."
    pip3 install -r requirements.txt
    '''
}

def deploy(env, port) {
    bat """
    echo "Cloning python-greetings repo..."
    if exist python-greetings rmdir /s /q python-greetings
    git clone https://github.com/mtararujs/python-greetings
    cd python-greetings
    echo "Stopping existing service if exists..."
    pm2 list | findstr "greetings-app-${env}" > nul
    if %errorlevel% neq 0 (
        echo "Process greetings-app-${env} not found, skipping delete."
    ) else (
        pm2 delete greetings-app-${env}
    )
    echo "Starting service on port ${port}..."
    pm2 start app.py --name greetings-app-${env} -- --port ${port} --no-daemon
    """
}


def test(env) {
    bat """
    echo "Cloning JS API test framework..."
    git clone https://github.com/mtararujs/course-js-api-framework
    cd course-js-api-framework
    echo "Installing Node dependencies..."
    npm install
    echo "Running tests on ${env} environment..."
    npm run greetings greetings_${env}
    """
}

pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }
    stages {
        stage('Check npm'){ 
            steps {
                script {
                    echo 'Checking npm version...'
                    bat 'cmd /c chcp 65001 && npm -v'
                }
            }
        }

        stage('Install pm2'){ 
            steps {
                script {
                    bat 'npm install -g pm2'
                }
            }
        }




        stage('install-pip-deps') {
            steps {
                script {
                    echo '=== Stage: install-pip-deps ==='
                    installPythonDeps()
                }
            }
        }

        stage('deploy-to-dev') {
            steps {
                script {
                    echo '=== Stage: deploy-to-dev ==='
                    deploy('dev', 7001)
                }
            }
        }

        stage('tests-on-dev') {
            steps {
                script {
                    echo '=== Stage: tests-on-dev ==='
                    test('dev')
                }
            }
        }

        stage('deploy-to-staging') {
            steps {
                script {
                    echo '=== Stage: deploy-to-staging ==='
                    deploy('staging', 7002)
                }
            }
        }

        stage('tests-on-staging') {
            steps {
                script {
                    echo '=== Stage: tests-on-staging ==='
                    test('staging')
                }
            }
        }

        stage('deploy-to-preprod') {
            steps {
                script {
                    echo '=== Stage: deploy-to-preprod ==='
                    deploy('preprod', 7003)
                }
            }
        }

        stage('tests-on-preprod') {
            steps {
                script {
                    echo '=== Stage: tests-on-preprod ==='
                    test('preprod')
                }
            }
        }

        stage('deploy-to-prod') {
            steps {
                script {
                    echo '=== Stage: deploy-to-prod ==='
                    deploy('prod', 7004)
                }
            }
        }

        stage('tests-on-prod') {
            steps {
                script {
                    echo '=== Stage: tests-on-prod ==='
                    test('prod')
                }
            }
        }
    }
}
