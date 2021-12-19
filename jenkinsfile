pipeline {
    agent any
    environment {
        my_access_key = credentials('aws_access_key')
        my_secret_key = credentials('aws_secret_key')
        my_security_token = credentials('aws_security_token')
    }
    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', credentialsId: 'StumptownRider-GitHub-PAT', url: 'https://github.com/StumptownRider/AnsibileCICD.git'
            }
        }
        stage('Run playbook') {
            steps {
                ansiblePlaybook( 
                playbook: 'ansDebugExample.yml'
                colorized: true,
                vaultCredentialsId: 'AnsibleVaultPassword',
                extras: '-e env1=$aws_security_token' 
                ) 
                ansiblePlaybook(
                playbook: 'ec2_newKeypair.yml',
                vaultCredentialsId: 'AnsibleVaultPassword',
                extras: '-e @aws-secret-vars.yml' 
                )
            }
        }
    }
}