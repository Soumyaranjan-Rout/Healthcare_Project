pipeline {
    agent any
    stages {
        stage('Build Maven') {
            steps {
                git url: 'https://github.com/Soumyaranjan-Rout/demo1.git', branch: 'master'
                sh 'mvn clean install'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t soumyaranjanrout0/app:1.0 .'
                }
            }
        }
        stage('Docker Login and Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push soumyaranjanrout0/app:1.0'
                }
            }
        }
        stage('Deploy to k8s') {
            when {
                branch 'master' // This is more reliable in declarative pipelines
            }
            steps {
                script {
                    kubernetesDeploy(configs: 'deploymentservice.yaml', kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}
