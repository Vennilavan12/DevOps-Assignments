pipeline {
    agent any
    environment {
      DockerImage= "vennilavan/dev-assesment"
    }
    stages {
       stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Vennilavan12/DevOps-Assignments.git']])
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'npm install'
                    sh 'npm test'
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    sh "docker build -t $DockerImage ."
                    sh "docker login -u $Docker_user -p $Docker_pass"
                    sh "docker push $DockerImage"
                }
            }
        }
        stage('Docker Deploy') {
            steps {
                script {
                    sh "docker run -itd -p 80:3000 $DockerImage"
                }
            }
        }
    }
    post {
      success {
            mail bcc: '', body: '"Good news! The pipeline completed successfully.\\nCheck build details at: ${env.BUILD_URL}', cc: '', from: '', replyTo: '', subject: 'Jenkins Job Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}', to: 'vennilavan98@gmail.com'
      }
    }
}
