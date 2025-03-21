# Installing and setting up Kubernetes Minikube

## Update system packages and Install dependencies
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl wget apt-transport-https conntrack socat
```
![image](https://github.com/user-attachments/assets/0019f77d-60d9-44fb-9cd9-030f88256d20)

## Install Docker, Kubectl and Minikube
### Install Docker
```bash
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
docker --version
```
### Install Kubectl
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client
```
### Install Minikube
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube version
```
![Screenshot from 2025-03-21 09-53-21](https://github.com/user-attachments/assets/add83714-cc8c-4e65-8b9d-1d6e8a08c4ea)

### Fix Docker setup
```bash
sudo usermod -aG docker $USER
reboot
```

### Verify Installation after reboot
```bash
docker run hello-world
```
![image](https://github.com/user-attachments/assets/7f55cdfd-b47d-4fec-93c5-59bebbeee6fb)


## Start and open Minikube
```bash
minikube start --driver=docker
minikube dashboard
```
![Screenshot from 2025-03-21 09-53-21](https://github.com/user-attachments/assets/2e7eac12-ca97-4ba8-ae66-a8524207f4f5)

![Screenshot from 2025-03-21 09-55-31](https://github.com/user-attachments/assets/6f2eba08-329f-4f6e-8f46-8cf67041acd3)


## Deployinig the docker image from dockerhub

```bash
mkdir docker
nano  Dockerfile
npm init -y
```

![Screenshot from 2025-03-20 15-53-09](https://github.com/user-attachments/assets/3d368ca1-cd54-4ee6-a2c5-6119c727e7dc)

```bash
npm init -y // again if the previous command installed npm
cd ..
docker pull tharun19stk/docker_stk:latest . // replace with your docker uid/repo:image_tag
cd docker
```

![Screenshot from 2025-03-20 15-53-53](https://github.com/user-attachments/assets/8ff07c3c-6001-4219-ab91-44d5adeb9543)


```bash
docker build -t tharun19stk/docker_stk/latest // replace with your docker uid/repo:image_tag
docker ps -a
```

![Screenshot from 2025-03-20 15-53-53](https://github.com/user-attachments/assets/11e2305c-ce60-4121-8b7a-82bbc6414d06)


```bash
sudo nano nginx-deployment.yaml
kubectl apply -f nginx-deployment.yaml
sudo nano service.yaml
kubectl apply -f service.yaml
kubectl get pods
minikube get svc my-app
minikube service my-app --url
curl <url>
```

![Screenshot from 2025-03-21 09-38-34](https://github.com/user-attachments/assets/b4a10411-4046-4a1c-bf8a-47d51d1a1543)


 - open the url in browser

![Screenshot from 2025-03-20 15-54-18](https://github.com/user-attachments/assets/5cc41ade-f9e6-4a30-9193-9c1a7d796f2a)

