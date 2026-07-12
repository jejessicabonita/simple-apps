pipeline {
    agent { label 'devops-jessica1' }
    
    tools {nodejs "NodeJS-18"}
environment {
        NAMEAPPS = 'simple-apps-pipeline-apps'
        SONARHOST = 'http://172.23.11.116:9000'
        TOKENSONAR = 'squ_d549846cb4d11396be0eb52af26fa580bbc4d1fa'
        VERSION = 'v1'
}    
    stages {
        stage('Build') {
            steps {
                sh '''
                npm install'''
            }
        }
        stage('Testing') {
            steps {
                sh '''
                npm test'''
            }
        }
        stage('Code Review') {
            steps {
                sh '''
                sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=${SONARHOST} \
                -Dsonar.login=${TOKENSONAR}'''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                docker compose down
                docker compose up -d
                '''
               }
        }
        stage('Tagging and Push to Registery Image') {
            steps {
                sh '''
                docker tag ${NAMEAPPS} jejessicabonita/${NAMEAPPS}:${VERSION}
                docker push jejessicabonita/${NAMEAPPS}:${VERSION}
                docker image prune -a -f
                '''
            }
        }
    }
}
