pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '15'))
        disableConcurrentBuilds()
        retry(2)
        timeout(time: 20, unit: 'MINUTES')
    }

    parameters {
        string(name: 'BRANCH', defaultValue: 'main', description: 'branch to build?')
        choice(name: 'ENV', choices: ['qa', 'dev', 'prod'], description: 'env to build')
    }

    stages {

       stage('Checkout Code') {
    steps {
        git branch: 'main',
            credentialsId: 'github-creds',
            url: 'https://github.com/vishalrathi00/vishalrathi00.git'
    }
}
        }

        stage('Docker Login, Build & Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'vishal9756',
                        passwordVariable: 'Circle9012'
                    )
                ]) {
                    sh '''
                        docker login -u "$vishal9756" -p "$Circle9012"
                        cd vote
                        docker build -t vishalrathi00/vishalrathi00.git/vote:v${BUILD_NUMBER} .
                        docker push docker push vishal9756/vote:v${BUILD_NUMBER}
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying to ${ENV}"
            }
        }
    }
}
