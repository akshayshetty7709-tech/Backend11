pipeline {
    agent any
    
    stages {
        stage('Deploy to EC2') {
            steps {
                sshagent(['Corzaar-creds']) {
                    withCredentials([file(credentialsId: 'env-corzaar-backend', variable: 'ENV_FILE')]) {

                    sh '''
                        scp -o StrictHostKeyChecking=no "$ENV_FILE" ubuntu@15.252.59.30:/home/ubuntu/.env.tmp
                        ssh -A -o StrictHostKeyChecking=no ubuntu@15.252.59.30 "
                            set -e && \\
                            rm -rf /home/ubuntu/Backend && \\
                            git clone -b main git@github.com:Corzaar/Backend.git && \\
                            cd /home/ubuntu/Backend && \\
                            docker build -t cz-backend . && \\
                            docker stop corzaar || true && \\
                            docker rm corzaar || true && \\
                            mv /home/ubuntu/.env.tmp .env && \\
                            docker run -itd -p 8000:8000 --env-file .env --network corzaar-network --name corzaar cz-backend
                        "
                    '''
                    }
                }
            }
        }
    }
}
