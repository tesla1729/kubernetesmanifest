pipeline {
agent {
        kubernetes {
            label 'jenkins-jenkins-agent'
        }
    }
  parameters {
       choice choices: ['abc', 'def', 'ghi'], description: 'select the service name in which to change the tag', name: 'servicename'
       string defaultValue: 'latest', description: 'add the tag for the service', name: 'servicetag'
       string defaultValue: '-release', description: 'add the tagsufix ', name: 'tagsuffix'
     }
  stages {
    stage('Clone') {
      steps {
          git branch: 'main', changelog: false, poll: false, url: 'https://github.com/tesla1729/kubernetesmanifest.git'
        }
    }  
    stage('Install yq') {
            steps {
                sh 'curl -LO https://github.com/mikefarah/yq/releases/download/v4.12.2/yq_linux_amd64.tar.gz'
                sh 'tar -xvf yq_linux_amd64.tar.gz'
                sh 'mv yq_linux_amd64 /usr/local/bin/yq'
                sh 'chmod +x /usr/local/bin/yq'
            }
        }
      stage('Trigger ManifestUpdate') {
      steps {
        script {
          catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        sh "git config user.email vanaparthy.rajkumar@gmail.com"
                        sh "git config user.name tesla1729"
                        //sh "git switch master"
                        sh "cat deployment.yaml"
                        sh "yq eval '.\"${params.servicename}\".image.tag = \"${params.servicetag}\"' deployment.yaml -i"
                        sh "yq eval '.\"${params.servicename}\".image.tagSuffix = \"${params.tagsuffix}\"' deployment.yaml -i"
                        sh "cat deployment.yaml"
                        sh "git add ."
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${BUILD_NUMBER}, updated service: ${params.servicename} tag: ${params.servicetag} tagsuffix: ${params.tagsuffix}'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
      }
        }
              }}
    }
}
}
