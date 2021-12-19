pipeline {
    agent any
    environment {
        my_access_key = credentials('aws_access_key')
        my_secret_key = credentials('aws_secret_key')
        my_security_token = credentials('aws_security_token')
    }
    stages {
        stage('Run playbook') {
            steps {
                ansiblePlaybook( 
                playbook: 'ansDebugExample.yml',
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