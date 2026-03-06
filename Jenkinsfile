pipeline {
    agent {
        node {
            label 'agent-1'
        }
    }
    environment {
        packageVersion = ''

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
    
            stage('git clone') {
                steps {
                    echo 'Building........'
                }
            }
            stage('Test') {
                steps {
                    echo 'Testing.......'
                }
            }
            stage('Deploy') {
                steps {
                    sh """
                        echo "Hello I am from shell script"
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
        }
        failure {
            echo 'failure occured, inform the concerned teams over slack'
        }
        success {
            echo 'Confragulations on success build'
        }
    }
}