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
                withCredentials([usernamePassword(credentialsId: 'kcldockerhub', passwordVariable: 'password', usernameVariable: 'uname')]) {
                    sh 'docker build -t mostafiz51/cicd-kcl:1 .'
                    sh 'docker build -t mostafiz51/cicd-kcl:latest .'

                    // Use Docker token from environment variable
                    sh "echo '${password}' | docker login -u ${uname} --password-stdin"
                    sh 'docker info'
                    sh 'docker push mostafiz51/cicd-kcl:1'
                    sh 'docker push mostafiz51/cicd-kcl:latest'

                     // Stop and remove any container running on the same port
                    sh '''
                    docker ps -q --filter "ancestor=mostafiz51/cicd-kcl:1" | xargs -r docker stop
                    docker ps -aq --filter "ancestor=mostafiz51/cicd-kcl:1" | xargs -r docker rm
                    '''

                    sh 'docker run -d -p 8070:80 mostafiz51/cicd-kcl:1'
                }
                           
            }
        }
    }
}
