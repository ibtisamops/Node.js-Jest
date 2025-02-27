pipeline{
    agent any

    tools {
        nodejs 'nodejs23'
    }


    environment {
        SCANNER_HOME = tool 'sonar-scanner-tool'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ibtisam-iq/Agri2Ops.git'
            }
        }
        stage('Build') {
            steps {
                dir('SonarQube/Nodejs-jest') {
                    nodejs('nodejs23') {
                        sh 'npm install'
                    }    
                }
            }    
        }
        stage('Test') {
            steps {
                dir('SonarQube/Nodejs-jest') {
                    nodejs('nodejs23') {
                        sh 'npm run test' // test is the script name in package.json, that's it is written as npm `run` test
                    }
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                dir('SonarQube/Nodejs-jest') {
                    withSonarQubeEnv('sonar-server') {
                        sh "${SCANNER_HOME}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
