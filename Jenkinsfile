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
                    def dockerrm = "sudo docker rm -f My-first-containe2211 || true"
                    def dockerCmd = "sudo docker run -itd --name My-first-containe2211 -p 8083:80 rushi1027/akshatnewimg6july:v1"

                    sshagent(['sshkeypair']) {
                        //change the private ip in below code
                        // sh "docker run -itd --name My-first-containe2111 -p 8083:80 akshu20791/2febimg:v1"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.33.193 ${dockerrm}"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.33.193 ${dockerCmd}"
                    }
                }
            }
        }
    }
}


