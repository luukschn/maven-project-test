pipeline {
    agent any

    // parameters {
    //     string(name: 'tomcat_dev', defaulvalue: 'localhost:8090', description: 'Staging Server')
    //     string(name: 'tomcat_prod', defaultvalue: 'localhost:9090', description: 'Production Server')
    // }
    //for deployment to running foreign server instance

    tools {
        maven 'localMaven'
    }

    environment {
        TOMCAT_URL_DEV = 'localhost:8090'
        TOMCAT_USER = 'tomcat'
        TOMCAT_PW = 'tomcat'
        CONTEXT_PATH = '/webapp'
    }

    triggers {
        pollSCM ('* * * * *') //CHANGE so i doenst poll every minute
    }

    stages {
        stage('Build') {
            steps {
                bat 'mvn clean package'
            }

            post {
                success {
                     echo 'Now Archiving'
                     archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deployments') {
            parallel {
                stage('Deploy to Staging') {
                    steps {
                        // def warFile = bat(script: 'dir /B /S target\\*.war', returnStatus: true).trim()
                        // powershell "Invoke-RestMethod -Uri \"${TOMCAT_URL_DEV}/manager/text/deploy?path=${CONTEXT_PATH}\" -Method Put -InFile \"${warFile}\" -Credential (Get-Credential -UserName ${TOMCAT_USER} -Password ${TOMCAT_PW})"
                        // powershell "Invoke-RestMethod -Uri \"${TOMCAT_URL_DEV}/manager/text/deploy?path=${CONTEXT_PATH}\" -Method Put -InFile \"**/target/*.war\" -Credential (Get-Credential -UserName ${TOMCAT_USER} -Password ${TOMCAT_PW})"
                    
                    
                        // bat "scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -P 22 **/target/*.war ${TOMCAT_USER}@${TOMCAT_URL_DEV}/manager"

                        // bat "ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -p 22 ${TOMCAT_USER}@${TOMCAT_URL_DEV} '${TOMCAT_MANAGER_PATH}/deploy?path=/your-app'"


                        //not so great way to do this
                        bat 'xcopy **/target/*.war C:/Users/luuks/Documents/Programming/jenkins_prac/apache-tomcat-10.1.16-staging/apache-tomcat-10.1.16/webapps'
                    }
                }
            }
        }
    }
}