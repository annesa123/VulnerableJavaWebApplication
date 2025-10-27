pipeline {
    agent { label 'built-in' }

    environment {
        REPORT_DIR = "dependency-check-report"
        DEPENDENCY_CHECK_IMAGE = "owasp/dependency-check:latest"
    }
        
    stages {
        stage('Dependency Check') {
            steps {
                sh '''
                    echo "=== Jalankan OWASP Dependency Check (All Formats) ==="
                    mkdir -p $REPORT_DIR
                    docker run --rm \
                        -v $(pwd):/src \
                        -v $(pwd)/$REPORT_DIR:/report \
                        $DEPENDENCY_CHECK_IMAGE \
                        --project "MyApp" \
                        --scan /src \
                        --format ALL \
                        --out /report
                '''
                echo "=== Report tersimpan di folder: $REPORT_DIR ==="
            }
        }
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
