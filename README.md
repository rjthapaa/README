
## Setting up node-todo-application in Jenkins with the help of Docker ##

Jenkins requires Java in order to run refer [link]([url](https://www.jenkins.io/doc/book/installing/linux/))

**Java installation commands** 
sudo apt update
sudo apt install openjdk-11-jre
java -version
openjdk version "11.0.12" 2021-07-20

**Jenkins installation Commands**
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
sudo systemctl status jenkins
sudo passwd ubuntu
sudo nano /etc/ssh/sshd_config
sudo systemctl restart sshd
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo usermod -aG jenkins $USER


**Docker and Docker-compose**
sudo apt install docker.io
sudo apt install docker-compose
sudo usermod -aG docker jenkins
sudo usermod -aG docker $USER
sudo reboot
docker ps

**** Configuring Jenkins **** 

1. Create a new item and select the free-style project and click on ok to create a project
2. Configure page opens up and in the General settings provide the project description and checkout github project checkbox and provide the github project url
3. Now, go to Source code management and select the git and provide the GitHub URL code of project. creds can be ognored it the project is public.
4. Uder build triggers section select it as GITHUB HOOK TRIGGER For GITSM POLLING (WebHook)  
6. Now, go to Build Steps and choose the execute shell build option.
7. Provide the docker commands to build an image from the docker file and to run the container.

$ docker build . -t nodeimage 
$ docker run --name=nodeC -d -p 8000:8000 nodeimage

6. Click on apply and save
7. Click on build now to execute the project.
8. Access the node application in web browser by providing <jenkinsPublicip:8000>

**** Initiallising Webhook ****

1. Webhook is a auto triggering tool, which traces all the new commits from GIT project and notifies the jenkins to build and run the applciation.
2. In order to activate the Webhook we need to add a plugin in Jenkins
3. Go to manage Jenkins and available plugins and search for GitHub integration Plugin and install it without restart.
4. Now go to github repo and click on setting to active webhook
5. Click on Webhook (will be on left hand side of the page) and in **payloadurl** add the jenkinsurl followed by github-webhook
htpp//:Jenkinspublicip/github-webhook 
6. Checkout send me everything option below
7. It should be configured now, check the webhooks section

**Webhook trigerring Jenkins**  

1. Make some changes to the file in GitHub for the project and commit it.
2. Monitor the build in Jenkins that must have triggered automatically by webhook.
3. Navigate to the URL to see the changes that you had made in the code in GitHub.

# Configuring tasks using Docker-Compose

1. Write a Docker Compose file for the application.
2. Now, go to Jenkins and navigate to the build steps of the project. Provide the docker-compose commands.

$ docker-compose down 
$ docker-compose up -d
echo " Container is sucess and running"

3. Click on apply and save
4. Click on build now to execute the project.
5. Access the node application in web browser by providing <jenkinsPublicip:8000>
