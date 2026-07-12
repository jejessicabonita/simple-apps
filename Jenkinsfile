pipeline {
    agent { label 'devops-jessica1' }
    tools {nodejs "NodeJS-18"}
    
    environment {
        NAMEAPPS = 'simple-apps-pipeline-apps'
        SONARHOST = 'http://172.23.11.116:9000'
        TOKENSONAR = 'sqp_95ac749c2dc6a5ab5ba426b84d308dde47697ff0'
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
                -Dsonar.host.url=http:${SONARHOST} \
                -Dsonar.login=${TOKENSONAR}'''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                docker compose -p simple-apps down
                docker compose -p simple-apps up -d --build
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
