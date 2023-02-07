pipeline {
    agent any

    stages {
        stage(' SCM Checkout') {

            steps {
                git 'https://github.com/fawzy1412/devopsproject1.git'
            }
        }

        stage(' Sending Dockerfile to Ansible ') {

            steps {
                sshagent(['ansible-demo']) {
                sh 'ssh -o StrictHostKeyChecking=no  ubuntu@172.31.16.47'
                sh 'scp /var/lib/jenkins/workspace/pipeline-demo-1/* ubuntu@172.31.16.47:/home/ubuntu  '
            }
            }
        }

        stage(' Build Docker Image at Ansible ') {

            steps {
                sshagent(['ansible-demo']) {
                sh 'ssh -o StrictHostKeyChecking=no  ubuntu@172.31.16.47 docker build -t $JOB_NAME:v1.$BUILD_NUMBER .  '
                
            }
            }
        }

        stage('  Image tagging ') {

            steps {
                sshagent(['ansible-demo']) {
                sh 'ssh -o StrictHostKeyChecking=no  ubuntu@172.31.16.47  cd /home/ubuntu '
                sh 'ssh -o StrictHostKeyChecking=no  ubuntu@172.31.16.47 docker image tag  $JOB_NAME:v1.$BUILD_NUMBER fawzy14/$JOB_NAME:v1.$BUILD_NUMBER '
                sh 'ssh -o StrictHostKeyChecking=no  ubuntu@172.31.16.47 docker image tag  $JOB_NAME:v1.$BUILD_NUMBER fawzy14/$JOB_NAME:v1.latest '
                
            }
            }
        }

        stage('  Push image ') {

            steps {
                sshagent(['ansible-demo']) {
                sh 'ssh -o StrictHostKeyChecking=no  ubuntu@172.31.16.47  cd /home/ubuntu '
                withCredentials([string(credentialsId: 'dockerhub_passwd', variable: 'dockerhub_passwd')]) {
                  sh 'ssh -o StrictHostKeyChecking=no  ubuntu@172.31.16.47  docker login -u fawzy14 -p ${dockerhub_passwd}' 
                  sh 'ssh -o StrictHostKeyChecking=no  ubuntu@172.31.16.47 docker image push  fawzy14/$JOB_NAME:v1.$BUILD_NUMBER '
                  sh 'ssh -o StrictHostKeyChecking=no  ubuntu@172.31.16.47 docker image push  fawzy14/$JOB_NAME:v1.latest '
                }
                
            }
            }
        }
        
       stage(' Sending deployment files to Kubernetes ') {

            steps {
                sshagent(['ansible-demo']) {
                sh 'ssh -o StrictHostKeyChecking=no  ubuntu@172.31.16.47 scp  /home/ubuntu/* ubuntu@172.31.31.245:/home/ubuntu'
              
            }
            }
        }










        stage('  apply  kubendeployment ') {

            steps {
                sshagent(['ansible-demo']) {
                sh 'ssh -o StrictHostKeyChecking=no  ubuntu@172.31.16.47   ansible-playbook /home/ubuntu/ansible-play.yml '
                
                }
                
            }
            }
        }







    }

