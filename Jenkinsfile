pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        // ansiColor('xterm')
    }
    environment {
        def appVersion = ''
    }
    stages {
        stage('read the version') {
            steps {
                script {
                    def packegJson = readJSON file: 'package.json'
                    appVersion = packegJson.version
                    echo "application version is $appVersion"
                }
            }
        }
        stage('install Dependencies') {
            steps {
                sh """
                 npm install
                 ls -ltr
                 echo "application version is $appVersion"
                """
                }
            }  
        }    
    
        post { 
            always { 
                echo 'I will always say Hello again!'
                deleteDir()
            }
            success { 
                echo 'I will run when pipeline is successful!'
            }
            failure { 
                echo 'I will run when pipeline is failed!'
            }
        }
    }
   
    
