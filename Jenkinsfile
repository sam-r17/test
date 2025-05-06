pipeline {
    agent any

    stages {
        stage('Prepare') {
            steps {
                sh 'git clone https://github.com/projectdiscovery/nuclei.git'
                sh 'apt install golang'
                sh 'cd nuclei/cmd/nuclei'
                sh 'go build'
                sh 'mv nuclei /usr/local/bin/'
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
