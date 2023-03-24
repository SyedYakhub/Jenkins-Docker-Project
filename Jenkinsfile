pipeline {
    agent any
    stages {
        stage ('git checkout'){
            steps {
                git 'https://github.com/SyedYakhub/Jenkins-Docker-Project.git'
            }
        }
        stage ('build docker image'){
            steps {
                sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                sh 'docker image tag $JOB_NAME:v1.$BUILD_ID yakhub4881/$JOB_NAME:v1.$BUILD_ID'
                sh 'docker image tag $JOB_NAME:v1.$BUILD_ID yakhub4881/$JOB_NAME:latest'
            }
        }
        stage ('push image to docker hub'){
            steps {
                    withCredentials([string(credentialsId: 'DockerPasswd', variable: 'DockerPasswd')]) {
                    sh "docker login -u yakhub4881 -p ${DockerPasswd}"
                    sh 'docker image push yakhub4881/$JOB_NAME:v1.$BUILD_ID'
                    sh 'docker image push yakhub4881/$JOB_NAME:latest'
                    sh 'docker image rmi $JOB_NAME:v1.$BUILD_ID yakhub4881/$JOB_NAME:v1.$BUILD_ID'

}
            }
        }
        stage ('docker container deployment'){
            steps {
                sshagent(['docker-based-webapp']) {
                sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.15.49 docker rm c1 -f'
                sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.15.49 docker image rmi yakhub4881/docker-based-webapp -f'
                sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.15.49 docker run -itd --name c1 -p 9000:80 yakhub4881/docker-based-webapp'
                }
            }
        }
    }
}
