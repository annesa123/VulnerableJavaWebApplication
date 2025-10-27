pipeline {
    agent { label 'built-in' }

    environment {
        REPORT_DIR = "dependency-check-report"
        DEPENDENCY_CHECK_IMAGE = "owasp/dependency-check:latest"
        NVD_API_KEY = credentials('NVD_API_KEY')
        DEP_DATA = "/var/jenkins_home/depcheck-data"
    }

    stages {
        stage('Dependency Check') {
            steps {
                sh '''
                    echo "=== Jalankan OWASP Dependency Check (cache & API key) ==="
                    mkdir -p $REPORT_DIR
                    mkdir -p $DEP_DATA
                    chmod 777 $DEP_DATA
                    docker run --rm \
                        -e NVD_API_KEY=$NVD_API_KEY \
                        -v $(pwd):/src \
                        -v $(pwd)/$REPORT_DIR:/report \
                        -v $DEP_DATA:/usr/share/dependency-check/data \
                        $DEPENDENCY_CHECK_IMAGE \
                        --project "MyApp" \
                        --scan /src \
                        --format ALL \
                        --out /report || true
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
