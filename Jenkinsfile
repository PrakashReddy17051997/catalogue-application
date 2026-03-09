pipeline {
    agent {
        node {
            label 'agent-1'
        }
    }
    environment {
        packageVersion = ''
        nexusUrl = '172.31.64.75:8081'

    }
    options {
        timeout(time: 1, unit:'HOURS')
        disableConcurrentBuilds()
    }
    stages{
        stage('Get Version'){
            steps{
                script{
                    def appVersion = readJSON file: 'package.json'
                    packageVersion = appVersion.version 
                    echo "application version: $packageVersion"
                }

            }
            }
    
            stage('Install Dependencies'){
                steps{
                    sh """
                        npm install
                    """                

            }
            }
            stage('Build'){
                steps{
                    sh """
                    ls -la
                    zip -q -r catalogue.zip ./* -x ".git" -x ".zip"
                    ls -ltr
                    """
                }
            }
            stage('Publish Artifact') {
            steps {
                 nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexusUrl}",
                    groupId: 'com.roboshop',
                    version: "${packageVersion}",
                    repository: 'catalogue',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                        [artifactId: 'catalogue',
                        classifier: '',
                        file: 'catalogue.zip',
                        type: 'zip']
                    ]
                )
            }
        }
            stage('Deploy') {
                steps {
                    sh """
                        echo "Hello I am deployment completed to nexus"
                        echo "$GREETING"
                        sleep 20
                        """
                }
            }
          
    }
    // post build
    post {
        always{
            echo 'We Will always Win again!'
            deleteDir()
        }
        failure {
            echo 'failure occured, inform the concerned teams over slack'
        }
        success {
            echo 'Confragulations on success build'
        }
    }
}