pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/agawand007/Ansible.git'
            }
        }
        
        stage('Install Ansible') {
            steps {
                sh 'sudo dnf -y install ansible-core'
            }
        }
        
        stage('Run Ansible Playbook') {
            steps {
                withCredentials([string(credentialsId: 'ansible-vault-password', variable: 'VAULT_PASS')]) {
                    sh '''
                        echo "$VAULT_PASS" > /tmp/.vault_pass
                        chmod 600 /tmp/.vault_pass
                        ansible-playbook -i inventory.ini lab5.yaml --vault-password-file=/tmp/.vault_pass
                        rm -f /tmp/.vault_pass
                    '''
                }
            }
        }
    }
    
    post {
        always {
            sh 'rm -f /tmp/.vault_pass'
        }
    }
}
