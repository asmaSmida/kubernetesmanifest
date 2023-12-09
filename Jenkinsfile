node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Update GIT') {
            script {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'githubid', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        sh "git config user.email asma.smida@insat.ucar.tn"
                        sh "git config user.name asma smida"
                        //sh "git switch master"
                        sh "cat deployment.yaml"
                        sh "sed -i 's+asmasmida13/dockertp.*+asmasmida13/dockertp:${DOCKERTAG}+g' deployment.yaml"
                        sh "cat deployment.yaml"
                        sh "git add ."
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
      }
    } 
  }
}
    stage('Deploy to Minikube') {
        steps {
            script {
                // Set the kubectl context to Minikube
                sh "kubectl config use-context minikube"

                // Apply the Kubernetes manifest
                sh "kubectl apply -f deployment.yaml"
            }
        }
}
