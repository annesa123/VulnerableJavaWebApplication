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
                script {
                    // Ambil tool Dependency-Check dari konfigurasi global
                    def dcHome = tool name: 'DependencyCheck', type: 'org.jenkinsci.plugins.DependencyCheck.tools.DependencyCheckInstallation'
                    if (!dcHome) {
                        error "Dependency-Check tool not found. Pastikan sudah dikonfigurasi di Manage Jenkins → Tools."
                    }

                    withCredentials([string(credentialsId: 'NVD_API_KEY', variable: 'API_KEY')]) {
                        sh """
                            "${dcHome}/bin/dependency-check.sh" \
                            --project "MyProject" \
                            --scan "." \
                            --format "ALL" \
                            --out "${REPORT_DIR}" \
                            --nvdApiKey \$API_KEY
                        """
                    }
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
