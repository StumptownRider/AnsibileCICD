pipeline {
    agent any
    // environment {
    //     my_access_key = credentials('aws_access_key')
    //     my_secret_key = credentials('aws_secret_key')
    //     my_security_token = credentials('aws_security_token')
    // }
    stages {
        stage('Build and package application') {
            steps {
                ansiblePlaybook(
                playbook: 'mvnPkg.yml'
                )
            }
        stage('Generate AWS Keypair') {
            steps {
                ansiblePlaybook(
                playbook: 'ec2_newKeypair.yml',
                vaultCredentialsId: 'AnsibleVaultPassword',
                extras: '-e @aws-secret-vars.yml' 
                )
            }
        stage('Launch EC2 Instance') {
            steps {
                ansiblePlaybook(
                playbook: 'env_ec2_create_savehost.yml',
                vaultCredentialsId: 'AnsibleVaultPassword',
                extras: '-e @aws-secret-vars.yml' 
                )
            }
        stage('Provision Tomcat') {
            steps {
                ansiblePlaybook(
                playbook: 'tomcat.yml',
                inventory: 'hosts.ini'
                )
            }
        stage('Deploy Application') {
            steps {
                ansiblePlaybook(
                playbook: 'copy_webapp.yml',
                inventory: 'hosts.ini'
                )
            }
        }
    }
}