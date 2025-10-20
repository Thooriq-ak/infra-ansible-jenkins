pipeline {
    agent any

    environment {
        ANSIBLE_FORCE_COLOR = 'true'
    }

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main', url: 'https://github.com/Thooriq-ak/infra-ansible-jenkins.git'
            }
        }

        stage('Print Working Directory') {
            steps {
                sh '''
                    echo "📂 Workspace path:"
                    pwd
                    echo ""
                    echo "📁 File list:"
                    ls -lah
                '''
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                sshagent(['ansible-ssh-key']) {
                    sh '''
                        echo "🚀 Running Ansible playbook..."
                        export ANSIBLE_HOST_KEY_CHECKING=False
                        ansible-playbook -i host.ini deploy.yml
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment succeeded!'
        }
        failure {
            echo '❌ Deployment failed. Check the Ansible logs above.'
        }
    }
}
