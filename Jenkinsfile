node {
    def app
    def acrHost = 'swkazurecr.azurecr.io'

    stage('Clone repository') {     
        checkout scm
    }

    stage('Update GIT') {
        script {
            withCredentials([usernamePassword(credentialsId: 'github_webhook', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
              
                sh "git config user.email demouser@gmail.com"
                
                sh "git config user.name demouser"
                
                sh "cat azure-vote-all-in-one-redis.yaml"

                sh "sed -i 's+${acrHost}/azure-vote-front.*+${acrHost}/azure-vote-front:${DOCKERTAG}+g' azure-vote-all-in-one-redis.yaml"
                sh "sed -i 's+${acrHost}/azure-vote-back.*+${acrHost}/azure-vote-back:${DOCKERTAG}+g' azure-vote-all-in-one-redis.yaml"
                sh "cat azure-vote-all-in-one-redis.yaml"
                
                sh "git add ."
				
                sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
				
                sh 'git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/voting-app-manifest.git HEAD:main'
            }                
        }
    }
}
