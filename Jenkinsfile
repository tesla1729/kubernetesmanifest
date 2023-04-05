pipeline {
    agent any
    parameters {
       choice choices: ['abc', 'def', 'ghi'], description: 'select the service name in which to change the tag', name: 'servicename'
       string defaultValue: 'latest', description: 'add the tag for the service', name: 'servicetag'
       string defaultValue: '-release', description: 'add the tagsufix ', name: 'tagsuffix'
     }
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
                sh "yq eval '.\"${params.servicename}\".image.tag = \"${params.servicetag}\"' deployment.yaml -i"
                sh "yq eval '.\"${params.servicename}\".image.tagSuffix = \"${params.tagsuffix}\"' deployment.yaml -i"
                sh 'cat deployment.yaml'
            }
        }
    }
}
