pipeline {
    agent any
    stages {
        stage('Clone project') {
            steps {
                echo 'Cloning the project...'
                git branch: 'main', url: 'https://github.com/sourcecode71/cicd-kcl.git'

                sh "ls -la"
            }
        }
        stage('Docker Stage') {
            steps {
                // Use environment variable for Docker token
                withCredentials([string(credentialsId: 'DOCKER_ACCESS_TOKEN', variable: 'DOCKER_TOKEN')]) {
                    sh 'docker build -t mostafiz51/cicd-kcl:1 .'
                    sh 'docker build -t mostafiz51/cicd-kcl:latest .'

                    // Use Docker token from environment variable
                    sh "echo '${DOCKER_TOKEN}' | docker login -u mostafiz51 --password-stdin"
                    sh 'docker info'
                    sh 'docker push mostafiz51/cicd-kcl:1'
                    sh 'docker push mostafiz51/cicd-kcl:latest'

                    sh 'docker run -d -p 8070:80 mostafiz51/cicd-kcl:1'
                }
            }
        }
    }
}
