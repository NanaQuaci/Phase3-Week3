pipeline {
    agent {
        label 'docker'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/NanaQuaci/Phase3-Week3.git'
            }
        }

        stage('Run JMeter Test') {
            steps {
                // Clean old reports if any exists
                sh '''
                rm -rf "${WORKSPACE}/reports" "${WORKSPACE}/results.jtl"
                mkdir -p "${WORKSPACE}/reports"
                '''


                // Run JMeter in non-GUI mode
                sh '''
                jmeter -n -t ${WORKSPACE}/Magento_Performance_Testing.jmx \
                                       -l ${WORKSPACE}/results.jtl \
                                       -e -o ${WORKSPACE}/reports
                '''
            }
        }

        stage('Verify JMeter Output') {
            steps {
                sh '''
                echo "Results file size:"
                ls -lh ${WORKSPACE}/results.jtl
                head -n 20 ${WORKSPACE}/results.jtl
                '''
            }
        }

        stage('Publish Report') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    keepAll: true,
                    reportDir: 'reports',
                    reportFiles: 'index.html',
                    reportName: 'JMeter HTML Report'
                ])
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'results.jtl, reports/**', fingerprint: true
        }
    }
}
