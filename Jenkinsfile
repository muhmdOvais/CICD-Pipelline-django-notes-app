pipeline{
    agent { label "ec2-agent"}

    stages{

        stage("Code"){
            steps{
                echo "This is cloning the code"

                git url: "https://github.com/muhmdOvais/CICD-Pipelline-django-notes-app.git", branch: "main"

                echo "Code cloning successful"
            }
        }

        stage("Build"){
            steps{
                echo "This is building the code"

                sh "docker build -t notes-app:latest ."
            }
        }

        stage("Push to DockerHub"){
            steps{

                echo "This is pushing the image to the dockerhub"

                withCredentials([usernamePassword(
                    credentialsId:"DockerhubCred",
                    passwordVariable:"DockerhubPass",
                    usernameVariable:"DockerhubUser"
                )]) {

                    sh "docker login -u ${env.DockerhubUser} -p ${env.DockerhubPass}"

                    sh "docker image tag notes-app:latest muhmdovais/notes-app:latest"

                    sh "docker push ${env.DockerhubUser}/notes-app:latest"
                }
            }
        }

        stage("Deploy"){
            steps{

                echo "This is deploying the code"

                sh "docker-compose down || true"

                sh "docker-compose up -d --build"
            }
        }
    }
}
