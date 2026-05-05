pipeline {

    agent { label 'agent-1' }

    environment {
        DEPLOY_HOST = "172.31.43.10"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Gurraiah123/Auth-project.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    scp -o StrictHostKeyChecking=no \
                    target/auth-app-1.0.jar \
                    ubuntu@$DEPLOY_HOST:/home/ubuntu/

                    ssh -o StrictHostKeyChecking=no ubuntu@$DEPLOY_HOST << EOF
pkill -f auth-app-1.0.jar || true
nohup java -jar /home/ubuntu/auth-app-1.0.jar > /home/ubuntu/app.log 2>&1 &
exit
EOF
                '''
            }
        }

    }

    post {
        success {
            echo 'Deployment successful'
        }

        failure {
            echo 'Deployment failed'
        }
    }

}
