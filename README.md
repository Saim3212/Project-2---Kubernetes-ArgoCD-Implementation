# Project-2---Kubernetes-ArgoCD-Implementation

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

