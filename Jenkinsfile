pipeline{
    agent any
    stages{
        stage("clean-up"){
            steps{
                sh "docker rm -f flask-app || true"
            }
        }
        stage("fs security scan"){
            steps{
                sh "trivy fs --format json -o trivy-result.json ."
            }
            post{
                always{
                    archiveArtifacts artifacts: "trivy-result.json", onlyIfSuccessful: true 
                }
            }
        }
        stage("build images"){
            steps{
                sh "docker build -t flask-app ."
            }
        }
        stage("run the container"){
            steps{
                sh "docker run -d -p 80:5500 --name flask-app flask-app"
            }
        }
    }
}