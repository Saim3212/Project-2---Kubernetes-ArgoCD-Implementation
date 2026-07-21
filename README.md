
This Project is an entirety of implementation of Kubernetes with ArgoCD along with the updates sent to the email. The tools used will be Jenkins , SonarQue , Docker , Trivvy, Grafana and Prometheus

<img width="1948" height="1149" alt="image" src="https://github.com/user-attachments/assets/8088884f-07fb-46ed-920e-c9d1137ef59f" />


 ## $${\color{Red} \textbf{Creation of An Ec2 Instance} \ \}$$

<img width="719" height="1185" alt="image" src="https://github.com/user-attachments/assets/fad5e1c1-0b64-437d-b130-b67042711af4" />

We first create the Master Instance with the properties as the image above , m71i fle

Some basic Git commands are:
> [!NOTE]
> Ubuntu
> >
> m7i-flex.large
>
> Network check all 3 boxes
> 
> 30 gb Space

Custom TCP rules

<img width="1466" height="622" alt="image" src="https://github.com/user-attachments/assets/cda25a76-846b-43b0-bc4f-aa5b232dad0b" />


 ## $${\color{Red} \textbf{Jenkins Installation} \ \}$$
Visit Link
>
[Jenkins](https://www.jenkins.io/doc/book/installing/linux/)
>
>Or copy paste the commands
```
sudo apt update
sudo apt install fontconfig openjdk-21-jre
java -version
```
and
```
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins
```
<img width="1790" height="695" alt="image" src="https://github.com/user-attachments/assets/a1298617-12f8-4fd7-930f-0227a6cbc6e9" />

```
sudo systemctl enable jenkins
```
```
sudo systemctl start jenkins
```
```
sudo systemctl status jenkins
```
Use {Instance Ip} :8080

<img width="1013" height="476" alt="image" src="https://github.com/user-attachments/assets/70d631a6-862b-4447-aa1f-6e5c68f29e8a" />

Install recommendeed Plugins

<img width="1014" height="731" alt="image" src="https://github.com/user-attachments/assets/f3553ad0-f757-4cda-a3f5-db173e0d3a1e" />

Login and Install Custom Plugins as given below

<img width="723" height="535" alt="image" src="https://github.com/user-attachments/assets/0cf61452-c1af-4651-a5e2-4e0eb6578c8a" />

```
Pipeline: Stage View
```
```
Blue Ocean
```
```
SonarQube Scanner
```
```
OWASP Dependency Check
```


 ## $${\color{Red} \textbf{Docker Installation} \ \}$$
```
 sudo apt-get install docker.io -y
sudo usermod -aG docker ubuntu && newgrp docker
```
 ## $${\color{Red} \textbf{Cluster Creation} \ \}$$

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install
```
In CLI 
```
aws configure
```
and add the credentials of the new created user 

<img width="618" height="133" alt="image" src="https://github.com/user-attachments/assets/c3b66af8-ba39-4443-ad1e-7158c3e2a950" />
```
eksctl create cluster --name=wanderlust \
                    --region=ap-south-1 \
                    --version=1.30 \
                    --without-nodegroup
```


 ## $${\color{Red} \textbf{IAM user} \ \}$$
 <img width="2546" height="253" alt="image" src="https://github.com/user-attachments/assets/ae3e7f8f-a125-4451-90ba-6812852f5cbf" />

<img width="983" height="374" alt="image" src="https://github.com/user-attachments/assets/639598a8-74ce-47b4-8cc7-faad2b4f5d0e" />
```
Enable admin access
```
Create new access Key 
<img width="1140" height="797" alt="image" src="https://github.com/user-attachments/assets/087e3ea9-48ea-454e-ab84-fb9dbd752ff9" />
<img width="791" height="601" alt="image" src="https://github.com/user-attachments/assets/e3d14846-945e-4b07-abab-b399cd8f061d" />



## $${\color{Red} \textbf{Install Kubectl} \ \}$$

```
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client
```
## $${\color{Red} \textbf{Install Ekctl} \ \}$$

```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```
## $${\color{Red} \textbf{Install Trivvy} \ \}$$

```
sudo apt-get install wget apt-transport-https gnupg lsb-release -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update -y
sudo apt-get install trivy -y
```
## $${\color{Red} \textbf{Install SonarQube} \ \}$$

```
docker run -itd --name SonarQube-Server -p 9000:9000 sonarqube:lts-community
```
{  Instance ID }:9000

<img width="534" height="236" alt="image" src="https://github.com/user-attachments/assets/3abb6928-c6b9-40c5-8747-147058e99ab7" />

## $${\color{Red} \textbf{NodeGroup Creation} \ \}$$

Edit the commands according to the instance 

```
eksctl create nodegroup --cluster=wanderlust \
                     --region=ap-south-1 \
                     --name=wanderlust \
                     --node-type=m7i-flex.large \
                     --nodes=2 \
                     --nodes-min=2 \
                     --nodes-max=2 \
                     --node-volume-size=29 \
                     --ssh-access \
                     --ssh-public-key=wanderkey
```
## $${\color{Red} \textbf{Generate Fine Grain Token} \ \}$$

Go into settings of github and Developer Settings
Create new token with everything enabled
<img width="813" height="1197" alt="image" src="https://github.com/user-attachments/assets/75a32fc7-f37e-4ddc-b8b8-fb67aa4f6416" />

## $${\color{Red} \textbf{Jenkins Credentials} \ \}$$
Select Credentials from Settings of Jenkins
<img width="1275" height="690" alt="image" src="https://github.com/user-attachments/assets/e50df3fa-e13b-4730-b338-74ff188ebf01" />
Click Global under System
<img width="1304" height="544" alt="image" src="https://github.com/user-attachments/assets/dd74fbee-23d0-41da-bd65-8d5506ee125d" />
Select username and password and add in the fine grained token as password
<img width="1717" height="975" alt="image" src="https://github.com/user-attachments/assets/751f3d34-237b-40b6-9c8d-4f07be0484b2" />
<img width="810" height="399" alt="image" src="https://github.com/user-attachments/assets/6db8f3a3-0afd-41fc-b910-f85748c436cb" />






