pipeline {
    agent any
    
    parameters {
        string(name: 'USERNAME', defaultValue: 'newuser', description: 'Username to create on the servers')
        text(name: 'SSH_PUBLIC_KEY', defaultValue: '', description: 'SSH Public Key to add for the user')
    }

    environment {
        ANSIBLE_INVENTORY = 'inventory.ini'
        ANSIBLE_PLAYBOOK = 'create_user_with_ssh.yml'
    }

    stages {
        stage('Checkout') {
            steps {
                // Check out the repository containing your playbook and inventory file
                git url: 'https://github.com/yashjskj/ansible-usercreate'
            }
        }

        stage('Install Ansible') {
            steps {
                sh 'sudo apt-get update && sudo apt-get install -y ansible'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                ansiblePlaybook playbook: "${ANSIBLE_PLAYBOOK}",
                                inventory: "${ANSIBLE_INVENTORY}",
                                extraVars: [
                                    new_user: "${params.USERNAME}",
                                    ssh_public_key: "${params.SSH_PUBLIC_KEY}"
                                ]
            }
        }
    }
    
    post {
        always {
            // Clean up workspace
            cleanWs()
        }
    }
}

