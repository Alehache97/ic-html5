pipeline {
    environment {
        TOKEN = credentials('SURGE_TOKEN')
      }
    agent {
        docker { image 'josedom24/debian-npm'
        args '-u root:root'
        }
    }
    stages {
        stage('Clone') {
            steps {
                git branch:'master',url:'https://github.com/Alehache97/ic-html5.git'
            }
        }
        
        stage('Install surge')
        {
            steps {
                sh 'npm install -g surge'
            }
        }
        stage('Deploy')
        {
            steps{
                sh 'surge ./_build/ macale.surge.sh --token $TOKEN'
            }
        }
        
    }
}
