@Library('jenkins-ansible-shared-lib') _

pipeline {
    agent any

    stages {

        stage('Approval') {
            steps {
                input message: 'Approve deployment to prod?',
                      ok: 'Proceed'
            }
        }

        stage('Ansible Execution') {
            steps {
                sh '''
                ansible-playbook -i inventory.ini playbook.yml
                '''
            }
        }
    }

    post {
        success {
            slackSend channel: 'jenkins-alert',
                      message: '✅ Consul deployment successful'
        }
        failure {
            slackSend channel: 'jenkins-alert',
                      message: '❌ Consul deployment failed'
        }
    }
}

