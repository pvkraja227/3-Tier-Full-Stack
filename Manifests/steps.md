https://www.youtube.com/watch?v=Nclhs-pRidM
3-Tier Ultimate DevOps CICD Pipeline Project | DevOps Project

App: container OR cloud
DB: MongoDB

Adv's AND Disadv's:

Container: 

1. Deployment-pods
2. Service
3. PV
4. PVC
5. Payment - yes

Cloud:

1. No Database needed, we can use that url to deploy on k8s or on EKS
2. No need to manage the pods and services, PV or PVC
3. Can watch everything over the UI
4. Payment - yes (comparatively cloud is best, bcz minimal handling)

3 - TIER:

user - repo - 3 tier

1. user - repo - Dependencies - nodejs - local deployment
2. user - repo - Jenkins - Dependencies - nodejs (test) - sonar - trivy FS - build - trivy Image - push - deploy to container
3. user - repo - Jenkins - Dependencies - nodejs (test) - sonar - trivy FS - build - trivy Image - push - AWS EKS

After deployment, check the logs to confirm whether we are able to connect to database or not !!

local: t2.med

sudo apt update

install nodejs:

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash (paste 3 lines from "export ...completion")
nvm install 21
node -v
npm -v

Google: cloudinary signup
nodejs/in Upload, Optimize and Transform / we have first 3 details: copy

basically we need the following:

CLOUDINARY_CLOUD_NAME=dcuh8288h
CLOUDINARY_KEY=448792691758224
CLOUDINARY_SECRET=XDC9VloBEF56vKdi1sVd0IPCjgM
MAPBOX_TOKEN=pk.eyJ1IjoicHZrcmFqYSIsImEiOiJjbTRkZmNmc2YwamVyMmtzYTdyNGs4aWhpIn0.dwgBYOuU_yYCxz9N0rkvJQ
DB_URL="mongodb+srv://pvkraja:kWDeNWr5iG3rUPno@devops.j8ex0.mongodb.net/?retryWrites=true&w=majority&appName=devops"
SECRET=devops

chrome: mapbox
get started for free/signup/copy token

chrome: mongodb atlas/MongoDB Atlas/signup/MO Free
name: devops/Mumbai/create deployment
username: pvkraja
pwd: kWDeNWr5iG3rUPno
create database user/choose a connection method/drivers/copy url (use quotes)
main page/scroll down ../network access/Add IP address/0.0.0.0/0 / confirm

secret: you can put any random value (devops)

ec2/

git clone https://github.com/pvkraja227/3-Tier-Full-Stack.git
cd 3-Tier-Full-Stack.git
npm install (install all dependencies to run the app)
vi .env (paste all 5 variables)/save
npm start (database connected)
check: publicIP:3000 / register / any user/email/pwd .. home
again goto atlas mongodb/view monitoring/collections/test/users and sessions created/new user is created

APP DEPLOYMENT ON DEV ENVIRONMENT:

t2.large - jenkins
t2.med - sonar

Jenkins:

sudo apt update
install java17: type java/install java17
install Jenkins - install Jenkins on ubuntu
sudo systemctl enable Jenkins
sudo systemctl start Jenkins
chrome: publicIP:8080

sudo cat /var/lib/Jenkins/.... (get admin pwd)/install plugins)

chrome: install docker on ubuntu: 2 steps
sudo usermod -aG docker Jenkins
to reflect the changes: go to browser /restart
(sudo chmod  666 /var/run/docker.sock)

Install Trivy

SonarQube:

sudo apt update
chrome: install docker on ubuntu: 2 steps
sudo usermod -aG docker $USER (or ubuntu)
newgrp docker OR OR
(sudo chmod  666 /var/run/docker.sock)

docker run -d -p 9000:9000 sonarqube:lts-community
docker ps (container running)
chrome: publicIP:9000 (admin/admin)

token: squ_ef9d8f1a9020bb06301ec80b95e940dd37066c0a

Jenkins dashboard:

Manage Jenkins/Plugins

nodejs
SonarQube scanner
pipeline stage view
docker pipeline
docker
Kubernetes
Kubernetes cli

Manage Jenkins/tools

add SonarQube scanner/name: sonar-scanner/version: latest
nodejs/name: node21/version: 21.7.0
docker/name: docker/install automatically/download from docker.com/ apply

manage jenkins/Credentials:

Global/add credentials
SonarQube username/token/ID: sonar-token and in desc
username and password/docker-cred

Manage Jenkins/system: (where we configure servers)

SonarQube servers/add sonar/name: sonar/copy url no "/"  / select sonar-token / save

pipeline/dev_environment_3tier
old builds/2
pipeline script - Jenkins-Dev
buildnow

chrome: localpublicIp:3000 / view camp grounds ../goto atlas mongodb/clusters/view monitoring/collections/test (all details are found ex: campgrounds, reviews, sessions, users etc..)
create one more user/new camp ground/camp-3/Germany/555/ddhhthht/add image ../ 3 images are available (bcz we have added maptool box)
add multiple users/multiple campgrounds/reviews .. all reflect in Atlas MongoDB

we deployed the app to dev envi, too see all the functionalities are working fine ..!!

Setting up EKS: 

AWS CLI Installation

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install

kubectl Installation

curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client 

eksctl Installation

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version 

create IAM/eks-user

policies:

amazonec2fullaccess
amazoneks_cni_policy
amazoneksclusterpolicy
amazoneksworkernodepolicy
awscloudformationfullaccess
iamfullaccess
eks-req (customer inline)

{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "VisualEditor0",
			"Effect": "Allow",
			"Action": "eks:*",
			"Resource": "*"
		}
	]
}

goto security credentials: create accesskey

aws configure 

Creating EKS Cluster

eksctl create cluster --name=my-eks22 \
--region=ap-south-1 \
--zones=ap-south-1a,ap-south-1b \
--without-nodegroup (we'll add nodegroup later, takes 10 min)

eksctl utils associate-iam-oidc-provider \
--region ap-south-1 \
--cluster my-eks22 \
--approve (to create certain resources in AWS)

eksctl create nodegroup --cluster=my-eks22 \
--region=ap-south-1 \
--name=node2 \
--node-type=t3.medium \
--nodes=3 \
--nodes-min=2 \
--nodes-max=3 \
--node-volume-size=20 \
--ssh-access \
--ssh-public-key=venkata \
--managed \
--asg-access \
--external-dns-access \
--full-ecr-access \
--appmesh-access \
--alb-ingress-access (takes 10 min)

kubectl get nodes - 3

eks/goto networking/security group - 2/ select: Additional Security group (Communication between the control plane and worker nodegroups)/click
goto inbound rules/edit/all traffic/0.0.0.0/0 / save

now RBAC

create svc, role, role binding,secret in Jenkins server: all.yml

kubectl create namespace webapps
vi svc.yml
kubectl apply -f svc.yml
vi role.yml
kubectl apply -f role.yml
vi bind.yml
kubectl apply -f bind.yml
vi secret.yml
kubectl apply -f secret.yml

kubectl describe secret mysecretname -n webapps
copy token

manage Jenkins/credentials/k8-token (paste)

CLOUDINARY_CLOUD_NAME=dcuh8288h
CLOUDINARY_KEY=448792691758224
CLOUDINARY_SECRET=XDC9VloBEF56vKdi1sVd0IPCjgM
MAPBOX_TOKEN=pk.eyJ1IjoicHZrcmFqYSIsImEiOiJjbTRkZmNmc2YwamVyMmtzYTdyNGs4aWhpIn0.dwgBYOuU_yYCxz9N0rkvJQ
DB_URL="mongodb+srv://pvkraja:kWDeNWr5iG3rUPno@devops.j8ex0.mongodb.net/?retryWrites=true&w=majority&appName=devops"
SECRET=devops

encode all values and store in repo/manifests

echo dcuh8288h | base64
remaining all - change

change docker image as well in repo/manifests

(liveness probe: accept traffic or not
readiness probe: if container is ready or not)

pipeline/prod_environment_3tier
old builds/2
pipeline script - Jenkins-Prod
buildnow

copy the externalIP and paste in chrome

kubectl get all -n webapps
kubectl describe pod podname -n webapps
kubectl logs podname -n webapps (database connected)

