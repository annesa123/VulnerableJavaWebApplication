pipeline {
    agent { label 'built-in' }

    environment {
        REPORT_DIR = "dependency-check-report"
        DEPENDENCY_CHECK_IMAGE = "owasp/dependency-check:latest"
        NVD_API_KEY = credentials('NVD_API_KEY')
    }

    stages {
        stage('Dependency Check') {
            steps {
                sh '''
                    echo "=== Jalankan OWASP Dependency Check (Dengan NVD API Key) ==="
                    mkdir -p $REPORT_DIR
                    docker run --rm \
                        -e NVD_API_KEY=$NVD_API_KEY \
                        -v $(pwd):/src \
                        -v $(pwd)/$REPORT_DIR:/report \
                        -v /var/jenkins_home/depcheck-data:/usr/share/dependency-check/data \
                        $DEPENDENCY_CHECK_IMAGE \
                        --project "MyApp" \
                        --scan /src \
                        --format ALL \
                        --out /report
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t vlun-java-apps:1.0 .'
            }
        }

        stage('Run Docker') {
            steps {
                sh '''
                    docker rm -f vlun-java-apps || true
                    docker run -d --name vlun-java-apps -p 9000:9000 vlun-java-apps:1.0
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'dependency-check-report/*.*', fingerprint: true
        }
    }
}
