pipeline {
    agent { label 'node-agent' }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/aj-c64/node-todo-cicd.git', branch: 'master'
            }
        }

        stage('Install') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh '''
                    if npm run | grep -q "^  test$"; then
                        npm test
                    else
                        echo "No test script found."
                    fi
                '''
            }
        }

        stage('Package Artifact') {
            steps {
                sh 'tar -czf node-app.tar.gz --exclude=node_modules --exclude=.git .'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'node-app.tar.gz', fingerprint: true
            }
        }
    }
}
