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
                        --nvdApiKey \$API_KEY
                    """
                }
            }
        }

        stage('Publish Report') {
            steps {
                publishHTML([
                    reportDir: "${REPORT_DIR}",
                    reportFiles: 'dependency-check-report.html',
                    reportName: 'Dependency Check Report',
                    allowMissing: false,
                    keepAll: true,
                    alwaysLinkToLastBuild: true
                ])
            }
        }
    }
}
