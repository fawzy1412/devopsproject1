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
                sh 'ssh -o StrictHostKeyChecking=no  ubuntu@54.164.112.251'
                sh 'scp /var/lib/jenkins/workspace/pipeline-demo-1/* ubuntu@54.164.112.251:/home/ubuntu'
            }
            }
        }


    }
}
