pipeline {
    agent any

    stages {
        stage('Prepare') {
            steps {
                sh 'go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest'
                sh 'alias nuclei=/var/jenkins_home/go/bin/nuclei'
                sh '/var/jenkins_home/go/bin/nuclei -update-templates'
                sh '/var/jenkins_home/go/bin/nuclei -version'
            }
        }
        stage('Scan') {
            steps {
                sh '/var/jenkins_home/go/bin/nuclei -u https://datasirpi.com -t ~/nuclei-templates -o results.txt'
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
