pipeline {
    agent any
    
    stages {
        stage('Setup') {
            steps {
                sh 'npm install -g newman newman-reporter-htmlextra'
            }
        }
        
        stage('Run API Tests') {
            steps {
                sh '''
                  mkdir -p newman/reports
                  npx newman run collections/PetShopApi.postman_collection.json \
                    -e environments/PetShopApi.postman_environment.json \
                    --reporters cli,junit,htmlextra \
                    --reporter-junit-export newman/reports/junit-report.xml \
                    --reporter-htmlextra-export newman/reports/html-report.html
                '''
            }
        }
    }
    
    post {
        always {
            junit 'newman/reports/junit-report.xml'
            publishHTML([
                reportDir: 'newman/reports',
                reportFiles: 'html-report.html',
                reportName: 'Newman HTML Report'
            ])
        }
        failure {
            echo 'Tests failed! Check the reports.'
        }
        success {
            echo 'All tests passed!'
        }
    }
}
