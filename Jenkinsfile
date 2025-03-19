pipeline {
    agent any
    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://github.com/Rushi2710/php-project/', branch: "master"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t rushi1027/akshatnewimg6july:v1 .'
                    sh 'docker images'
                }
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh '''
                        echo "$PASS" | docker login -u "$USER" --password-stdin
                        docker push rushi1027/akshatnewimg6july:v1
                    '''
                }
            }
        }

        stage('Deploy to Server') {
            steps {
                script {
                    def remoteHost = "ubuntu@172.31.33.193"
                    def dockerrm = "docker rm -f My-first-containe2211 || true"
                    def dockerCmd = "docker run -itd --name My-first-containe2211 -p 8083:80 rushi1027/akshatnewimg6july:v1"

                    sshagent(['sshkeypair']) {
                        sh """
                            ssh -o StrictHostKeyChecking=no ${remoteHost} '${dockerrm}'
                            ssh -o StrictHostKeyChecking=no ${remoteHost} '${dockerCmd}'
                        """
                    }
                }
            }
        }
    }
}


