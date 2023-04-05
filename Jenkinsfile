pipeline {
    agent any
    stages {
        stage('Install yq') {
            steps {
                sh 'curl -LO https://github.com/mikefarah/yq/releases/download/v4.12.2/yq_linux_amd64.tar.gz'
                sh 'tar -xvf yq_linux_amd64.tar.gz'
                sh 'mv yq_linux_amd64 /usr/local/bin/yq'
                sh 'chmod +x /usr/local/bin/yq'
            }
        }
        stage('Edit YAML File') {
            steps {
                sh 'yq --version'
                sh 'yq eval \'abc.image.tag = "2.2.2"\' deployment.yaml -i'
                sh 'yq eval \'def.image.tag = "3.3.3"\' deployment.yaml -i'
                sh 'cat deployment.yaml'
            }
        }
    }
}
