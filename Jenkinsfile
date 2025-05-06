pipeline {
    agent any

    stages {
        stage('Prepare') {
            steps {
                sh 'nuclei -update-templates'
            }
        }
        stage('Scan') {
            steps {
                sh 'nuclei -l urls.txt -t ~/nuclei-templates -o results.txt'
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
