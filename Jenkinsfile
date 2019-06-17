pipeline {
    agent any
    tools {        
        nodejs "NodeJS 12.3.1"
    }
    stages { 
        stage('Checkout') {
            git 'https://github.com/arperrin/sample-angular.git'
        }
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
                    env.VERSION = json.version + "-${BUILD_NUMBER}"
                    env.ARTIFACT="${JOB_BASE_NAME}-" + json.version + ".tar.gz" 

                    sh'''
                        cd dist
                        tar -cvf ${ARTIFACT} sample-angular/.
                    '''               

                    nexusArtifactUploader artifacts: [[artifactId: 'sample-angular', classifier: '', file: "dist/${ARTIFACT}", type: 'tar.gz']], 
                        credentialsId: 'ada569da-ce50-4f7a-b09f-eb643c48521b', 
                        nexusUrl: '192.168.33.10:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: 'perrin', 
                        version: "${VERSION}"
                }               
            }
        }
    }
}