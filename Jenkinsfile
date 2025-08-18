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
                sh 'sudo docker build -t myapp:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'DOCKERHUB_PASS')]) {
                    sh '''
                    echo "Logging into dockerhub"
                    echo "$DOCKERHUB_PASS" | sudo docker login -u $dockerhub-pass --password-stdin
                    sudo docker tag myapp:latest purval1992/maven-project-pipeline:0.1
                    sudo docker push purval1992/maven-project-pipeline:0.1
                    '''
                }
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh 'ansible-playbook -i inventory deploy.yml'
            }
        }
    }
}
