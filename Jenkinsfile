pipeline {
    agent { label 'built-in' }
    tools {
    'dependency-check' 'DependencyCheck'
    }

    environment {
        REPORT_DIR = "dependency-check-report"
    }

    stages {
        stage('Run OWASP Dependency-Check') {
            steps {
                withCredentials([string(credentialsId: 'NVD_API_KEY', variable: 'API_KEY')]) {
                    sh """
                        dependency-check \
                        --project "VulnerableJavaWebApplication" \
                        --scan "." \
                        --format "ALL" \
                        --out "${REPORT_DIR}" \
                        --nvdApiKey \$API_KEY \
                        --data "dependency-check-data" \
                        --disableOssIndex true \
                        --failOnCVSS 0.0
                    """
                    archiveArtifacts artifacts: 'dependency-check-report/dependency-check-report.html'
                    archiveArtifacts artifacts: 'dependency-check-report/dependency-check-report.json'
                    archiveArtifacts artifacts: 'dependency-check-report/dependency-check-report.xml'
                }
            }
        }
    }
}
