pipeline {
    agent any
    
    stages {
        stage('Deploy to EC2') {
            steps {
                sshagent(['Corzaar-creds']) {

                    sh '''
                        ssh -A -o StrictHostKeyChecking=no ubuntu@15.252.59.30 "
                            set -e && \
                            rm -rf /home/ubuntu/Backend && \
                            git clone -b git@github.com:Corzaar/Backend.git && \
                            cd /home/ubuntu/Backend && \
                            docker build -t cz-backend .
                            docker run -itd -p 8000:8000 --name corzaar cz-backend
                        "
                    '''
                }
            }
        }
    }
}
