pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()   // Jenkins Workspace Cleanup Plugin
            }
        }
        
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/purvalpatel/Sample_Nginx_Project_autodevops.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'sudo docker build --no-cache -t myapp:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pass', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
                    sh '''
                    echo "Logging into dockerhub"
                    echo "$DOCKERHUB_PASS" | sudo docker login -u purval1992 --password-stdin
                    sudo docker tag myapp:latest purval1992/maven-project-pipeline:0.2
                    sudo docker push purval1992/maven-project-pipeline:0.2
                    '''
                }
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh 'ansible-playbook -i /etc/ansible/inventory/hosts.ini deploy.yaml'
            }
        }
    }
}
