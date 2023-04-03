pipeline {
agent {
        kubernetes {
            label 'jenkins-jenkins-agent'
        }
    }
  parameters {
    string(name: 'DOCKERTAG', defaultValue: 'latest', description: 'Image tag')
   }
  stages {
    stage('Clone') {
      steps {
          git branch: 'main', changelog: false, poll: false, url: 'https://github.com/tesla1729/kubernetesmanifest.git'
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
                        sh "sed -i 's+teslaraj950/testing-image.*+teslaraj950/testing-image:${params.DOCKERTAG}+g' deployment.yaml"
                        sh "cat deployment.yaml"
                        sh "git add ."
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                        sh "cat deployment.yaml"
                        sh "echo 'https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
      }
        }
              }}
    }
}
}
