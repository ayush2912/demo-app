pipeline {
    agent any 
    environment{
        SONAR_HOME= tool "Sonar"
    }
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/ayush2912/demo-app", branch: "main"
            }
        }
        
        stage("code quality"){
            steps{
                withSonarQubeEnv("Sonar"){
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=demo-app -Dsonar.projectKey=demp-app"
                }
            }
        }
        
        
            
        stage("Build"){
            steps {
                echo "Building the image"
                sh "docker build -t demo-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag demo-app ${env.dockerHubUser}/demo-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/demo-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
                
            }
        
            
        }
    }
    post {
     success {
        mail to: 'ayushvashistha567@gmail.com',
             subject: 'Pipeline Succeeded',
             body: 'Jenkins pipeline completed successfully.'
    }
    failure {
        mail to: 'ayushvashistha567@gmail.com',
             subject: 'Pipeline Failed',
             body: 'Jenkins pipeline failed.'
    }
  }
}
