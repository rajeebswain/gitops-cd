pipeline{
    agent any
    environment {
        APP_NAME = "gitops-demo-app"
       }
    stages {
        stage('Clean Workspace'){
            steps{
                script{
                    cleanWs ()
                }
            }
        }
        stage('Checkout SCM'){
            steps{
                git credentialsId: 'github', 
                url: 'https://github.com/rajeebswain/gitops-deployment.git',
                branch: 'main'
            }
        }
        stage('Updating Kubernetes deployment file'){
            steps{
               sh "cat deployment.yml"
               sh "sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml"
               sh "cat deployment.yml"
            } 
        }
        stage('Push the changed deployment file to Git'){
            steps {
                script{
                    sh """
                    git config --global user.name "rajeebswain"
                    git config --global user.email "rajeeb.mop@gmail.com"
                    git add deployment/deployment.yml
                    git commit -m 'Updated the deployment file' """
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'pass', usernameVariable: 'user')]) {
                        sh "git push https://$user:$pass@github.com/rajeebswain/gitops-deployment.git mainr"
                    }
                }
            }     
        }
    }
}
