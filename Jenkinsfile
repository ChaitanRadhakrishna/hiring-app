pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'github-account', url: 'https://github.com/ChaitanRadhakrishna/hiring-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t chaitan121/chaitan_docker:$BUILD_NUMBER .'
                }
            }
        }
        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'DockerHub_Account', url: 'https://index.docker.io/v1/') {
                    sh 'sleep 5'
                    sh 'docker push chaitan121/docker_hub:$BUILD_NUMBER'
                    }
                }
            }
        }
        stage('Checkout K8S manifest SCM'){
            steps {
              git branch: 'main', credentialsId: 'github-account', url: 'https://github.com/ChaitanRadhakrishna/hiring-app.git'
            }
        }
        stage('Update K8S manifest & push to Repo') {
            steps {
                script {
                withCredentials([string(credentialsId: 'github', variable: 'GITHUB_PAT')]) {
                sh '''
                    cat /var/lib/jenkins/workspace/$JOB_NAME/dev/deployment.yaml
                    sed -i "s/13/${BUILD_NUMBER}/g" /var/lib/jenkins/workspace/$JOB_NAME/dev/deployment.yaml
                    cat /var/lib/jenkins/workspace/$JOB_NAME/dev/deployment.yaml
                    git add .
                    git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                    git remote -v
                    git push https://github.com/betawins/hiring-app main
                '''
                    }
                }
            }
        }
    }
}
