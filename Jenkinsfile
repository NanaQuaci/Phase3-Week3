pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/NanaQuaci/Phase3-Week3.git'
            }
        }

        stage('Run JMeter Test') {
            steps {
                // Clean old reports if any
                sh 'rm -rf reports results.jtl || true'

                // Run JMeter in non-GUI mode
                sh '''
                jmeter -n -t Magento_Performance_Testing.jmx \
                       -l results.jtl \
                       -e -o reports
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
