pipeline {
    agent any

    tools {
        maven 'localMaven'
    }
    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deploy to staging'){
            steps {
                build job: 'deploy-to-staging-maven'
            }
        }

        stage('Deploy to prod'){
            steps {
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION deployment?'
                }

                build job: 'deploy-to-prod-maven'
            }

            post {
                success {
                    echo 'Code deployed to production.'
                }

                failure {
                    echo 'Deployment failed.'
                }
            }
        }
    }
}
