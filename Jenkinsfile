pipeline{
    agent any
   
   stages{
       stage("Clone Code"){
           steps{
                echo "Cloning the code from github"
                git url:"https://github.com/rajpreet88/django-notes-app.git", branch: "main"
           }
       }
       stage("Build"){
           steps{
               echo "Building the docker image"
               sh "docker build -t my-note-app ."
           }
       }
       stage("Push to Docker Hub"){
           steps{
               echo "Pushing the image to docker hub"
               withCredentials([usernamePassword(credentialsId:"dockerHub", passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")]){
                    sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/my-note-app:latest"
               }
           }
       }
       stage("Deploy"){
           steps{
               echo "Deploying the container"
            //   sh "docker run -d -p 8000:8000 rajpreet88/my-note-app:latest"
            sh "docker-compose down && docker-compose up -d"
           }
       }
   } 
   
}
