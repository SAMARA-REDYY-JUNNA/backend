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
        nexusUrl = 'nexus.mydaws.fun:8081'
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
        stage('built') {
            steps {
                sh """
                zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                ls -ltr
                """
                }
            } 

            stage('sonar scan'){
                environment {
                    scannerHome = tool 'sonar'
                }
                steps {
                    script {
                        withSonarQubeEnv('sonar'){
                            sh "${scannerHome}/bin/sonar-scanner"
                        }
                    }
                }
            }
            stage('Sonar Quality Gate') {
                steps {
                    timeout(time: 30, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        
            stage('Nexus Artifact Upload') {
            steps {
                script{
                     nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "${nexusUrl}",
                        groupId: 'com.expense',
                        version: "${appVersion}",
                        repository: "backend",
                        credentialsId: 'nexus-auth',
                        artifacts: [
                            [artifactId: "backend",
                            classifier: '',
                            file: "backend-" + "${appVersion}" + '.zip',
                            type: 'zip']
                        ]
             )
                }
                }
            } 
            stage('Deploy') {
                steps {

                    script {
                        def params = [
                        string(name: 'appVersion', value: "${appVersion}"),
                    ]
                        build job: 'backend-deploy', parameters: params, wait: false  
                    }
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

   
    
