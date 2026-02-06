pipeline {
    agent any

    stages {
        stage('Install Newman') {
            steps {
                sh 'npm install newman'
            }
        }

        stage('Run API Tests') {
            steps {
                sh '''
                  mkdir -p newman
                  npx newman run postman/PetShopApi.postman_collection.json \
                    -e postman/PetShopApi.postman_environment.json \
                    --reporters cli,junit \
                    --reporter-junit-export newman/PetShopApi.xml
                '''
            }
        }
    }

    post {
        always {
            junit 'newman/PetShopApi.xml'
        }
    }
}
