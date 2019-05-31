pipeline {
    agent any
    tools {        
        nodejs "NodeJS 12.3.1"
    }
    stages { 
        stage('Build') {
            steps {
                sh'''
                npm install && ng build --prod
                '''
            }
        }
        stage('Archive') {
            steps {
                sh'''
                ARCHIVE_NAME="${JOB_BASE_NAME}-${BUILD_NUMBER}.tar.gz"
                TARGET=dist/sample-angular/.
                tar -cvf ${ARCHIVE_NAME} ${TARGET}
                '''
            }
        }
    }
}