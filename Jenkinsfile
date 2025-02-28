pipeline {
    agent {
        docker {
            image 'josedom24/debian-npm'  // Usa la imagen de Docker que ya tiene Surge instalado
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
       
        // Se eliminó la etapa de cambiar repositorios a HTTPS ya que no es necesaria.
       
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
                    // Surge ya está instalado en la imagen Docker, por lo que no es necesario instalarlo
                    sh 'surge ./_build/ macalex.surge.sh --token $TOKEN'
                }
            }
        }
    }
}
