pipeline {
agent any


environment {
    DEPLOY_HOST = '192.168.1.21'
    DEPLOY_USER = 'vboxuser'
    DEPLOY_PATH = '/var/www/html'
}

stages {

    stage('Verify') {
        steps {
            echo 'Checking Student Portal files...'

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

                echo "Build completed successfully."
            '''
        }
    }

    stage('Test') {
        steps {
            echo 'Testing Student Portal...'

            sh '''
                test -f build/index.html
                test -f build/style.css
                test -f build/script.js

                echo "Website test passed."
            '''
        }
    }

    stage('Deploy') {
        steps {
            echo 'Deploying Student Portal to Deploy VM...'

            sh '''
                ssh -o StrictHostKeyChecking=no ${DEPLOY_USER}@${DEPLOY_HOST} "rm -rf ${DEPLOY_PATH}/*"

                scp -o StrictHostKeyChecking=no -r build/* ${DEPLOY_USER}@${DEPLOY_HOST}:${DEPLOY_PATH}/

                echo "Deployment completed successfully."
            '''
        }
    }
}

post {
    success {
        echo 'Student Portal deployed successfully!'
        echo "Website: http://${DEPLOY_HOST}"
    }

    failure {
        echo 'Student Portal deployment failed!'
        }
    }
}
