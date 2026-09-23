```groovy
pipeline {
    agent any

    environment {
        DEPLOY_HOST = '192.168.1.21'
        DEPLOY_USER = 'vboxuser'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
            }
        }

        stage('Verify Website') {
            steps {
                echo 'Checking website files...'

                sh '''
                    test -f index.html
                    test -f style.css
                    test -f script.js

                    echo "All website files are present."
                '''
            }
        }

        stage('Build') {
            steps {
                echo 'Building Student Portal...'

                sh '''
                    rm -rf build
                    mkdir build

                    cp index.html build/
                    cp style.css build/
                    cp script.js build/
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Testing website...'

                sh '''
                    test -f build/index.html
                    test -f build/style.css
                    test -f build/script.js

                    echo "Website test successful."
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying website to Deploy VM...'

                sh '''
                    scp -r build/* ${DEPLOY_USER}@${DEPLOY_HOST}:/var/www/html/
                '''
            }
        }
    }

    post {
        success {
            echo 'Student Portal deployed successfully!'
        }

        failure {
            echo 'Student Portal deployment failed.'
        }
    }
}
```
