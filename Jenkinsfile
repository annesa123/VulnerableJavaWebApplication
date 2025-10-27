pipeline {
    agent { label 'built-in' }
    tools {
        dependencyCheck 'DependencyCheck'  
    }

    environment {
        REPORT_DIR = "dependency-check-report"
        NVD_API_KEY = credentials('NVD_API_KEY')
    }

    stages {
        stage('Run OWASP Dependency-Check') {
            steps {
                script {
                    // ambil lokasi tool dari konfigurasi global Jenkins
                    def dcHome = tool 'DependencyCheck'
                    sh """
                        ${dcHome}/bin/dependency-check.sh \
                        --project "MyProject" \
                        --scan "." \
                        --format "ALL" \
                        --out "${REPORT_DIR}"
                        --nvdApiKey ${NVD_API_KEY}
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
