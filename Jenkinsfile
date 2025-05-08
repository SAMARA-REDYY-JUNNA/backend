pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        // ansiColor('xterm')
    }
    stages {
        stage('read the version') {
            steps {
                def packegJson = readJSON file: 'packeg.json'
                def appVersion = packegJson.version
                echo "application version is $appVersion"
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
   
    
