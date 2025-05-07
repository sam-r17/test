pipeline {
    agent any

    stages {
        stage('Prepare') {
            steps {
                sh '''
                go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
                alias nuclei=/var/jenkins_home/go/bin/nuclei
                /var/jenkins_home/go/bin/nuclei -update-templates
                /var/jenkins_home/go/bin/nuclei -version
                '''
            }
        }
        stage('Scan') {
            steps {
                sh '''
                    # Run nuclei with JSON output
                    /var/jenkins_home/go/bin/nuclei -u https://datasirpi.com -t ~/nuclei-templates -json-export results.json

                    # Extract non-informational findings
                    jq '.[] | select(.info.severity != "info")' results.json > findings.json

                    if [ -s findings.json ]; then
                      echo "Non-informational findings detected:"
                      cat findings.json
                      exit 1
                    else
                      echo "No significant findings."
                    fi
                '''
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
