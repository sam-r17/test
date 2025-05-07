pipeline {
    agent any

    stages {
        stage('Prepare') {
            steps {
                sh 'go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest'
                sh 'nuclei -update-templates'
                sh 'nuclei -version'
            }
        }
        stage('Scan') {
            steps {
                sh 'nuclei -u https://datasirpi.com -t ~/nuclei-templates -o results.txt'
            }
        }
        stage('Archive Results') {
            steps {
                archiveArtifacts artifacts: 'results.txt', fingerprint: true
            }
        }
    }
    post {
        always {
            echo "Scan complete."
        }
        failure {
            echo "Scan failed due to critical findings."
        }
    }
}
