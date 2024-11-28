pipeline{
    agent any
    stages{
        stage('checkout the code from github'){
            steps{
                 git url: 'https://github.com/Soumyaranjan-Rout/Medicare_Project.git'
                 echo 'github url checkout'
            }
        }
        stage('Maven codecompile'){
            steps{
                echo 'starting compiling'
                sh 'mvn compile'
            }
        }
        stage('Maven codetesting'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Maven qa'){
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('Maven package'){
            steps{
                sh 'mvn package'
            }
        }
        stage('Build Docker image'){
            steps{
                sh 'sudo docker build -t soumyaranjanrout0/medicure-image:v1 .'
            }
        }
        stage('Docker login'){
            steps{
                withCredentials([string(credentialsId: 'dockerhublogin', variable: 'dockerhublogin')]) {
                    sh 'docker login -u soumyaranjanrout0 -p ${dockerhublogin}'
                }
            }
        }
        stage('Docker Push Iamge'){
            steps{
                sh 'sudo docker push soumyaranjanrout0/medicure-image:v1'
            }
        }
        stage('Deploy using Ansible'){
            steps{
                ansiblePlaybook become: true, credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'Ansible-Playbook.yml', sudoUser: null, vaultTmpPath: ''
            }
        }
    }
}
