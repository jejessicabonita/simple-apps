pipeline {
    agent { label 'devops-jessica1' }
    
    tools {nodejs "NodeJS-18"}

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
                npm test
                npm run test:coverage'''
            }
        }
        stage('Code Review') {
            steps {
                sh '''
                sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.11.116:9000 \
                -Dsonar.login=sqp_95ac749c2dc6a5ab5ba426b84d308dde47697ff0'''
            }
        }
      stage('Push NPM') {
            steps {
                sh '''
                npm install
                npm test
                '''
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
                docker tag simple-apps-freestyle-apps jejessicabonita/simple-apps-freestyle-apps
                docker push jejessicabonita/simple-apps-freestyle-apps
                docker images prune -a -f
                '''
            }
        }
    }
}
