pipeline {
    agent any
    tools {        
        nodejs "NodeJS 12.3.1"
    }
    stages { 
        stage('Build') {
            steps{
                sh'''
                npm install && ng build --prod
                '''
            }
        }
    }
}