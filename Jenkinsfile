pipeline {
    agent any
    environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
                }
        stages {
            stage('Docker Build') {
                agent any
            steps {
                sh 'if [ $(docker ps -aq) ]; then docker ps -aq | xargs docker stop;fi'
                sh 'docker system prune -af --volumes'
                /*sh 'if [ -d "/usr/local/jenkins/Docker/testnginx" ]; then rm -R /usr/local/jenkins/Docker/testnginx/; fi'*/
                dir('/usr/local/jenkins/Docker') { 
                /*sh 'git clone https://github.com/lgandzii/testnginx'*/
                sh 'docker build /usr/local/jenkins/Docker/testnginx/ -t lgandzii/testnginx -f /usr/local/jenkins/Docker/testnginx/Dockerfile'
                sh 'docker run --name nginxtest -d -p 80:80 lgandzii/testnginx'
                                                 }
                }
                                 }
            stage('Dockerhub') {
                agent any
                steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                
                sh 'docker push lgandzii/testnginx:latest'
                }
                }
         }
}
