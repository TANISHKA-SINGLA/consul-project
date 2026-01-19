@Library('jenkins-ansible-shared-lib@main') _

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo "Ansible project already checked out"
                sh 'ls -la'
            }
        }

        stage('User Approval') {
            steps {
                input message: "Approve deployment to PROD?"
            }
        }

        stage('Ansible Execution') {
            steps {
                sh '''
                pwd
                ls -la
                ansible-playbook -i inventory.ini playbook.yml
                '''
            }
        }
	stage('Deploy') {
            steps {
                ansiblePipeline(
                    inventory: 'inventory.ini',
                    playbook: 'playbook.yml'
                )
            }
    }

    post {
        failure {
            slackSend channel: 'jenkins-alert',
                      message: "❌ Consul deployment failed"
        }
        success {
            slackSend channel: 'jenkins-alert',
                      message: "✅ Consul deployed successfully"
        }
    }
}
