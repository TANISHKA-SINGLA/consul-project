@Library('jenkins-ansible-shared-lib@main') _

pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo "Ansible project checked out by Jenkins"
                sh '''
                pwd
                ls -la
                '''
            }
        }

        stage('User Approval') {
            steps {
                input message: "Approve deployment to PROD?"
            }
        }

        stage('Deploy Consul') {
            steps {
                ansiblePipeline(
                    inventory: 'inventory.ini',
                    playbook: 'playbook.yml'
                )
            }
        }
    }

    post {
        success {
            slackSend channel: 'jenkins-alert',
                      message: "✅ Consul deployed successfully"
        }
        failure {
            slackSend channel: 'jenkins-alert',
                      message: "❌ Consul deployment failed"
        }
    }
}
