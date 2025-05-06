pipeline {
    agent any

    stages {
        stage('Prepare') {
            steps {
                sh 'git clone https://github.com/projectdiscovery/nuclei.git'
                sh 'cd nuclei/cmd/nuclei'
                sh 'go mod init example.com/nuclei'
                sh 'go build nuclei/cmd/nuclei/main.go'
                sh 'mv nuclei/cmd/nuclei/main /usr/local/bin/nuclei'
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
