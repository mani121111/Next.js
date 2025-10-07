pipeline {
    agent any

    tools {
        nodejs 'nodejs-18'  // Use the configured Node.js version
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/main']], 
                    extensions: [], 
                    userRemoteConfigs: [[url: 'https://github.com/prudhvisurya996/nginxwebdb.git']]
                ])
            }
        }
        stage('create a package.json file'){
            steps{
                sh 'npm init'
            }

        }
         stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('run the start script') {
            steps{
                sh 'npm start'
            }
        }
        stage('build the project'){
            steps{
                sh 'npm run build'
            }
        }
        stage('build and push to dockerHub'){
         steps{
            script{
                withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                    sh "docker build -t  MyNextJS ."
                    sh "docker tag myimg mani121/MyNextJS:latest"
                    sh "docker push mani121/MyNextJS:latest"
                }

            }
         }
        }
    }
}
