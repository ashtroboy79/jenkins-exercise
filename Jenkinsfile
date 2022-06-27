pipeline{
    agent any
    stages{
        stage('Clone'){
            steps{
                deleteDir()
                git 'https://gitlab.com/qacdevops/chaperootodo_client'
            }
        }
        stage("Install"){
            steps{
                sh 'curl https://get.docker.com | sudo bash'
                sh '''# make sure jq & curl is installed
                sudo apt update
                sudo apt install -y curl jq
                version=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | jq -r \'.tag_name\')
                sudo curl -L "https://github.com/docker/compose/releases/download/${version}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
                sudo chmod +x /usr/local/bin/docker-compose'''
            }
        }
        stage("Deploy"){
            steps{
                sh 'sudo docker-compose pull && sudo -E DB_PASSWORD=${DB_PASSWORD} docker-compose up -d'
            }
        }
    }
}
