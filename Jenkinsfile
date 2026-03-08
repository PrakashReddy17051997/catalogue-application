pipeline {
    agent {
        node {
            label 'agent-1'
        }
    }
    environment {
        packageVersion = ''
        nexusUrl = 'http://172.31.64.36:8081/repository/catalogue/'

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
                        groupID:'com.roboshop',
                        version: "${packageVersion}",
                        repository: 'catalogue',
                        credentailsId: 'nexus-auth',
                        artifacts: [
                            [artifactId: 'catalogue',
                            calssifier: '',
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
            stage('check params') {
                steps{
                    sh """
                        echo "Hello ${params.PERSON}"
                        echo "Biography: ${params.BIOGRAPHY}"
                        echo "Toggle: ${params.TOGGLE}"
                        echo "choice: ${params.CHOICE}"
                        echo "Password: ${params.PASSWORD}"
                        echo "trigger is added"
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