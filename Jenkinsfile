pipeline {
    environment {
        TOKEN = credentials('SURGE_TOKEN')
    }
    agent {
        docker { 
            image 'josedom24/debian-npm'
            args '-u root:root'
        }
    }
    stages {
        stage('Clone') {
            steps {
                git branch: 'master', url: 'https://github.com/Alehache97/ic-html5.git'
            }
        }

        stage('Check Connectivity') {
            steps {
                sh 'curl -I https://registry.npmjs.org'
            }
        }

        stage('Install npm & Surge') {
            steps {
                script {
                    sh 'npm cache clean --force'
                    sh 'npm install -g npm@latest'
                    sh 'npm install -g surge || npm install -g surge' // Intentar dos veces si falla
                }
            }
        }

        stage('Install Pip') {
            steps {
                script {
                    sh 'apt update -y && apt install -y python3-pip default-jre'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh 'pip install --upgrade pip'
                    sh 'pip install html5validator'
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
                sh 'surge ./_build/ aleherrera.surge.sh --token $TOKEN'
            }
        }
    }
}
