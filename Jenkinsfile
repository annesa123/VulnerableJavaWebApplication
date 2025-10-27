pipeline {
    agent { label 'built-in' }
    stages {
        stage('Docker Build') {
            steps {
                sh 'docker build -t vlun-java-apps:1.0 .'
            }   
        }
        stage('Run Docker') {
            steps {
                sh 'docker rm -f vlun-java-apps'
                sh 'docker run -itd --name vlun-java-apps -p 9000:9000 vlun-java-apps:1.0'
            }
        }
    }
}
