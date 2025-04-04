pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/SathwikPrince/webapp-automation.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'sudo apt update && sudo apt install -y python3 python3-pip ansible'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                sh 'ansible-playbook -i inventory ansible-playbook.yml'
            }
        }
        
        stage('Deploy to EC2') {
            steps {
                sshagent(['jenkins-ssh-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@44.203.201.75 <<EOF
                    cd /var/www/webapp-automation
                    sudo systemctl restart webapp
                    EOF
                    '''
                }
            }
        }
    }
}
