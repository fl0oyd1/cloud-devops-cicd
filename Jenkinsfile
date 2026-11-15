pipeline {
    agent any

    tools {
        nodejs 'NodeJS-7.8.0'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                sh 'CI=true npm test -- --watchAll=false'
            }
        }

        stage('Docker') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        env.IMAGE = 'nodemain:v1.0'
                        env.PORT = '3000'
                        env.CONTAINER = 'main-app'
                    } else {
                        env.IMAGE = 'nodedev:v1.0'
                        env.PORT = '3001'
                        env.CONTAINER = 'dev-app'
                    }
                }

                sh 'docker build -t $IMAGE .'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker rm -f $CONTAINER || true'
                sh 'docker run -d --name $CONTAINER -p $PORT:3000 $IMAGE'
            }
        }
    }
}
