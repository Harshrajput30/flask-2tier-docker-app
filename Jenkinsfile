pipeline{
    agent any;
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/Harshrajput30/flask-2tier-docker-app" , branch : "main"
            }
        }
        stage("build"){
            steps{
                sh 'docker build -t two-tier-flask-app .'
            }
        }
        stage("docker login"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]){
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }
        stage("push to dockerhub"){
            steps{
                sh 'docker tag two-tier-flask-app harshrajputt/two-tier-flask-app:latest'
                sh 'docker push harshrajputt/two-tier-flask-app:latest '
            }
        }
       stage("Deployment"){
            steps{
                sh 'docker rm -f flask-container || true'
                sh 'docker run -d -p 80:80 --name flask-container harshrajputt/two-tier-flask-app:latest'
            }
        }
    }
}
