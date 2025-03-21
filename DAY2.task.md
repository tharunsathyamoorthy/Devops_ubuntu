# Creating a docker image using 
## Installing Docker
  - Enter the following commands to inatall and verify installation of Docker
```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable docker --now
docker
```
![image](https://github.com/user-attachments/assets/e0a899be-cb40-40cf-9231-e35ce45a6e4f)
![image](https://github.com/user-attachments/assets/cbfdbef3-ad47-488b-89f5-0b450c27e3af)

## Download Docker plugins in Jenkins
 - Go to Jenkins `Dashboard` -> `Manage Jenkins` -> `Available Plugins` -> Search `Docker`
 - Select these plugins and Install
    - Docker
    - Docker Commons
    - Docker Pipeline
    - docker-build-step
    - CoudBees Docker Build and Publish

![423886665-5eef535f-2b65-44ca-9ac2-2a19548477c5](https://github.com/user-attachments/assets/a484e33e-926e-4bac-88dd-fb3ffc59da1d)
)
![423888873-3f4bda54-a3dc-4811-a546-b027d83a679f](https://github.com/user-attachments/assets/ec33ee4b-afad-421e-9b1c-cbbe447287d1)
)
![423889060-f27a51c0-f7d0-4ea2-808e-161a16d296d4](https://github.com/user-attachments/assets/7fa606ca-bd93-4c95-ad38-a302f23bd592)
)

 - In this page Check the `Restart Jenkins` after installation this will restart Jenkins

![image](https://github.com/user-attachments/assets/023e655e-e8e7-4b3b-9b74-317f9f4484f2)

## Add Jenkins to Docker group
 - Go to terminal and run these commands to add Jenkins to docker group
```bash
sudo usermod - aG docker jenkins
sudo systemctl restart jenkins
sudo reboot
```
- This will do its thing and reboot the system 

## Setting up docker credentials
 - Go to Jenkins > `Manage Jenkins` > `Credentials` > `System` > `Global Credentials (Unrestricted)` > `Add Credentials`
 -  Fill your Docker hub `username` , `password`, and in the `id` field enter `docker-seccred`
   
![Screenshot from 2025-03-20 09-21-08](https://github.com/user-attachments/assets/b5d73a82-9b1d-497d-8c5e-e95353769023)


## Creating and building a pipeline


 - Go to Jenkins `Dashboard` > `Create a Job`

![423894086-cd3138ce-07b3-4070-97b9-0a8d76df4a36](https://github.com/user-attachments/assets/75014e28-a454-481b-9f92-4e3f4784cfec)


 - Enter a project name 
 - Select `pipeline`
 - Click `Ok`

![Screenshot from 2025-03-20 11-18-16](https://github.com/user-attachments/assets/2b705db9-11f2-4ab8-8ae3-cb4c270539c8)


 - Go to `pipeline`
 - Paste this script below and change the credential wherever mentioned:
```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = "santhapriyan/docker"          // Replace with your Docker Hub username and image name
        TAG = "latest"
        CONTAINER_NAME = "my-container"
        PORT = "3001"
    }

    stages {
       
        stage('Clone Repository') {
            steps {
                echo "Cloning GitHub repository..."
                git branch:'main' ,url:'https://github.com/santhapriyan-s/docker.git'  // Replace with your repo URL
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh 'chmod +x build.sh'
                sh './build.sh'
            }
        }

                stage('Login to Docker Hub') {
            steps {
                echo "Logging into Docker Hub..." // Change the cedentialsID if you have docker credentials already added with another id other than docker-seccred
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "Pushing Docker image to Docker Hub..."
                sh "docker tag $IMAGE_NAME:$TAG $IMAGE_NAME:$TAG"
                sh "docker push $IMAGE_NAME:$TAG"
            }
        }

        stage('Deploy Docker Container') {
            steps {
                echo "Deploying Docker container..."
                sh 'chmod +x deploy.sh'
                sh './deploy.sh'
            }
        }
    }

    post {
        success {
            echo "Deployment Successful!"
        }
        failure {
            echo "Deployment Failed!"
        }
    }
}

```
 - click `save`
 - click `build`

![Screenshot from 2025-03-20 09-25-58](https://github.com/user-attachments/assets/dd7cf484-bd1a-4c11-a9fc-6c5c49bfe567)
)

 - Go to `localhosy:3001`
   
![Screenshot from 2025-03-20 11-32-32](https://github.com/user-attachments/assets/93eb1218-7d48-4b49-b583-7e10351e2d04)
)

