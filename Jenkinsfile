pipeline {
    agent {
        docker {
            image 'josedom24/debian-npm'
            args '-u root:root'
        }
    }
   
    environment {
        TOKEN = credentials('SURGE_TOKEN')
    }
   
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Alehache97/ic-html5.git'
            }
        }
       
        stage('Change Repositories to HTTPS') {
            steps {
                script {
                    sh """
                    sed -i 's/http:/https:/g' /etc/apt/sources.list
                    apt update
                    """
                }
            }
        }
       
        stage('Install Surge') {
            steps {
                script {
                    sh 'npm install -g surge'
                }
            }
        }
       
        stage('Install Pip') {
            steps {
                script {
                    sh 'apt update -y && apt install pip default-jre -y'
                }
            }
        }
       
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'pip install html5validator '
                }
            }
        }
       
        stage('Test HTML') {
            steps {
                script {
                    sh 'html5validator --root _build/'
                }
            }
        }
       
        stage('Deploy') {
            steps {
                script {
                    sh 'surge ./_build/ macalex.surge.sh --token $TOKEN'
                }
            }
        }
    }
}
