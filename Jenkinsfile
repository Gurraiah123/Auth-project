pipeline {

    agent { label 'build-agent' }

    stages {

        stage('Checkout') {
            steps {
                git 'YOUR_GITHUB_URL'
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
                scp target/auth-app-1.0.jar ubuntu@APP_SERVER_PRIVATE_IP:/home/ubuntu/

                ssh ubuntu@APP_SERVER_PRIVATE_IP "
                    pkill java || true

                    nohup java -jar /home/ubuntu/auth-app-1.0.jar > app.log 2>&1 &
                "
                '''
            }
        }

    }
}
