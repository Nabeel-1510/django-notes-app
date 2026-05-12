@Libraries
pipeline{

    agent { label 'vinod' }

    stages{
        stage("Hello"){
            steps{
                script{
                    hello()
                }
            }
        }
                

        stage("CODE"){

            steps{

                echo "Cloning code from GitHub"

                git url: "https://github.com/Nabeel-1510/django-notes-app.git", branch: "main"
            }
        }

        stage("BUILD IMAGE"){

            steps{

                echo "Building Docker image"

                sh "docker build -t notes-app:latest ."
            }
        }

        stage("PUSH IMAGE"){

            steps{

                echo "Pushing image to Docker Hub"

                withCredentials([usernamePassword(
                    credentialsId: "dockerhubCred",
                    usernameVariable: "dockerhubUser",
                    passwordVariable: "dockerhubPass"
                )]){

                    sh 'echo $dockerhubPass | docker login -u $dockerhubUser --password-stdin'

                    sh "docker tag notes-app:latest ${dockerhubUser}/notes-app:latest"

                    sh "docker push ${dockerhubUser}/notes-app:latest"
                }
            }
        }

        stage("DEPLOY"){

            steps{

                echo "Deploying application"

                sh "docker compose up -d"
            }
        }
    }
}
