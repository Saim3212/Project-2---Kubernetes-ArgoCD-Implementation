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


## $${\color{Red} \textbf{SonarQube} \ \}$$
<img width="1544" height="887" alt="image" src="https://github.com/user-attachments/assets/040efa89-2a1b-416d-9dee-76896d8b95ad" />
Generate and Copy the Token
<img width="1208" height="595" alt="image" src="https://github.com/user-attachments/assets/124412c5-4d3e-4334-89d5-817a87c2bb7c" />
Add Sonar as the ID to match the Jenkinsfile
<img width="546" height="465" alt="image" src="https://github.com/user-attachments/assets/d8954297-f285-4a98-bb2f-31bf265a7df4" />

Click on Administration>Configuration>Webhooks
<img width="1488" height="415" alt="image" src="https://github.com/user-attachments/assets/108afd4c-c8bc-4ebd-b557-be80389d2731" />
Add in Jenkins-Webhook, With Jenkins URL as the url 

<img width="456" height="542" alt="image" src="https://github.com/user-attachments/assets/40136d97-ac0d-4288-bad9-05698ed02fff" />


## $${\color{Red} \textbf{OWASP Setup} \ \}$$
manage Jenkins>Tools>Dependency Check
<img width="882" height="1262" alt="image" src="https://github.com/user-attachments/assets/a8e6be68-c714-4dbb-8de8-8d8728e53c85" />
```
OWASP
Make it install automatically
Github
```
Click Save
Request an NVD API Key from link
[NVD API KEY](https://nvd.nist.gov/developers/request-an-api-key)

Visit the Link provided after submiiting a key reqeust

<img width="1025" height="429" alt="image" src="https://github.com/user-attachments/assets/d4115c06-4569-49da-9f90-f62915064f2d" />

Copy paste the Key in Secret
<img width="1575" height="572" alt="image" src="https://github.com/user-attachments/assets/1580a729-c979-4a5a-a545-ca5589e5d4cd" />

<img width="500" height="455" alt="image" src="https://github.com/user-attachments/assets/5c04e350-9d6d-4ecb-9a37-d10a2e11d099" />





## $${\color{Red} \textbf{Generate DockerHub Token} \ \}$$
Go Settings in Dockerhub and geenrate new access token
<img width="2148" height="746" alt="image" src="https://github.com/user-attachments/assets/5a4be078-6956-4b4c-aa17-cc84ba23eedf" />
follow the specifics
<img width="712" height="523" alt="image" src="https://github.com/user-attachments/assets/f08c36f2-cc23-48e6-891b-3c6c635033f2" />
Copy the access Token that is given and paste it in the credentails Tab in Jenkins as Password
<img width="494" height="550" alt="image" src="https://github.com/user-attachments/assets/bc74c189-4555-4434-b544-ad34586a0c5b" />
make sure the username is the username in docker

## $${\color{Red} \textbf{VSCode} \ \}$$
Edit the file Provided within the Repo to match the instance ID of the wanderlust
<img width="1370" height="171" alt="image" src="https://github.com/user-attachments/assets/197a7773-af42-4801-88e5-57ad28e5e323" />

<img width="1122" height="524" alt="image" src="https://github.com/user-attachments/assets/24f299e6-fef6-4978-b726-e15d3e1034e0" />

Open Gitbash and login through command

```
aws configure
```
<img width="689" height="272" alt="image" src="https://github.com/user-attachments/assets/26c9030c-d19a-4dc7-9f71-efb13d8fbbbe" />

Once done , pass the command

```
bash updatefrontendnew.sh
```
and 
```
bash updatebackendnew.sh
```
Once commands passed , the automations should now show the instance IPs match the one in the dockerfile

<img width="1174" height="389" alt="image" src="https://github.com/user-attachments/assets/016f556a-9f00-486c-8947-cc6b010496bb" />
<img width="1329" height="277" alt="image" src="https://github.com/user-attachments/assets/84c44122-78e1-4a13-9f86-0af140cf40ca" />

## $${\color{Red} \textbf{ArgoCd Configure} \ \}$$

Installation Commands
```
kubectl create namespace argocd
```
```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
```
watch kubectl get pods -n argocd
```
```
sudo curl --silent --location -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/v2.4.7/argocd-linux-amd64
```
```
sudo chmod +x /usr/local/bin/argocd
```
```
kubectl get svc -n argocd
```
```
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
```
```
kubectl get svc -n argocd
```
Change the Inbound Rules in security for the 2 Wanderlust instances as well

<img width="657" height="453" alt="image" src="https://github.com/user-attachments/assets/f075382a-075a-403a-b4f7-2876d79ccf15" />
<img width="1613" height="484" alt="image" src="https://github.com/user-attachments/assets/5ac68283-8113-421b-b9b8-d2aa1b7462b1" />

To get password paste in 

```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

for adding Repo 
<img width="2032" height="370" alt="image" src="https://github.com/user-attachments/assets/d041fa89-a66c-422f-bddf-b3627179a192" />
Add in given options
<img width="908" height="817" alt="image" src="https://github.com/user-attachments/assets/0fc7da7e-fbdc-43b1-9ba6-fff90865e228" />

Add in SonarQube in Jenkins

Link in the URL from SonarQube
<img width="773" height="886" alt="image" src="https://github.com/user-attachments/assets/836473b3-683b-492d-8f41-f43fea832a57" />


```
```

```
```

```










