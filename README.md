
sudo dnf install wget -y

sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
    
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

sudo yum upgrade

# Add required dependencies for the jenkins package
sudo yum install java-11-openjdk

sudo yum install jenkins
sudo systemctl daemon-reload

### Start Jenkins

sudo systemctl enable jenkins

sudo systemctl start jenkins


sudo systemctl status jenkins


#  Plugins Needed

#### SSH
Publish Over SSH ,	
SSH Agent	,
SSH Pipeline Step,

GitLab,
Generic Webhook Trigger,

Kubernetes
Kubernetes CLI
Kubernetes :: Pipeline :: DevOps Steps

Docker,
Docker Pipeline,
docker-build-step,
