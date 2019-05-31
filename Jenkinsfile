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
        stage('Publish To Nexus') {
            steps {
                script {
                    def json = readJSON file: 'package.json'
                    env.VERSION = json.version                
                }
                withCredentials([usernamePassword(credentialsId: 'Nexus', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh'''
                        ARTIFACT="${JOB_BASE_NAME}-${VERSION}-${BUILD_NUMBER}.tar.gz"
                        REPOSITORY="http://192.168.33.10:8081/nexus/content/sitesn/Angular"
                        cd dist
                        tar -cvf ${ARTIFACT} sample-angular/.
                        curl -v -u ${user}:${pass} --upload-file ${ARTIFACT} ${REPOSITORY}/${VERSION}/${ARTIFACT}
                    '''               
                } 
            }
        }
    }
}