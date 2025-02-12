Jenkins CI/CD Pipeline for AWS Infrastructure  
Automated Terraform Deployment with Jenkins & Docker

📌 Project Overview
This project demonstrates how to **automate AWS infrastructure deployment** using **Jenkins running in Docker** to trigger Terraform scripts. The pipeline is designed to **create AWS networking resources dynamically** with infrastructure as code.

---

🛠 Tools & Technologies Used
- Terraform  (_Infrastructure as Code to provision AWS networking resources_)  
- AWS  (_VPC, Subnets, Internet Gateway, Transit Gateway, Route Tables, Security Groups, and EC2_)  
- GitHub  (_Source control for Terraform scripts, integrated with Jenkins via Webhooks_) 
- Docker  (_Containerized Jenkins environment for portability and automation_)
- Jenkins  (_Running in a Docker container locally to automate the pipeline_)   
- Jenkinsfile  (_Defines pipeline stages for infrastructure deployment_)  
  

**📂 Terraform Infrastructure Setup**

First Let's create our repo.
![Screenshot 2025-02-12 at 1 23 47 PM](https://github.com/user-attachments/assets/743a08dd-6065-411e-9ed2-3cab624cd6da)

Now we need to copy the url of our repo in the "Code" drop down. Then we can go into VScode and remote into the repo from our local device. The command we need to run in our VSCode terminal is "git remote add origin <your_repos_url>"
![Screenshot 2025-02-12 at 1 35 26 PM](https://github.com/user-attachments/assets/99ba00b6-e8c1-45e6-ab25-5c264dda207a)

Our terraform files will now be uploaded to our GitHub repo. We need to run the following commands:

"git init" (To start git on our local folder)

"git add ." (This will add all the files in our current working directory. If you don't want to add all files there are two ways to resolve this. We could use a .gitignore file[https://git-scm.com/docs/gitignore])

OR "git add <name_of_the_files_you_want_to_add" (Notice there is no "." after "add" this means we need to name them 1 by 1)

Next we need to run "git commit -m <Your_message_here>" (The message is one way we can keep track of the changes we make with each commit)

Lastly for this step "git push -u origin <name_of_your_branch>" 
![Screenshot 2025-02-12 at 1 49 10 PM](https://github.com/user-attachments/assets/c653d2dc-ed6e-48a2-bb7b-8afd3ea632b0)

Now our local files are now in our GitHub repository!!!


---

****Next lets set up our Jenkins instance using docker****

First make sure you have docker installed and have created an acccout[https://www.docker.com]
![Screenshot 2025-02-12 at 1 57 24 PM](https://github.com/user-attachments/assets/7b055325-f056-4a18-bd9e-7513bbc0bd7b)

Navigate to the "Docker Desktop" tab
![Screenshot 2025-02-12 at 1 58 33 PM](https://github.com/user-attachments/assets/de00e2f9-c5a8-4ae4-85fb-ef5dad913151)

Open Docker desktop
![Screenshot 2025-02-12 at 1 59 40 PM](https://github.com/user-attachments/assets/42653677-b443-42cc-b5e4-f9bbb9ac74a9)

Now we need to run a command in terminal to *pull* the docker image for Jenkins. We will run the command "docker pull jenkins/jenkins:latest"
![Screenshot 2025-02-12 at 2 04 46 PM](https://github.com/user-attachments/assets/02efd45c-7b44-4a92-8861-5cc41a061cc8)

We now have to most current image of Jenkins on our machine. We now need to to start the image as a container. 
Lets run the following command "docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:latest"

Key point!: " -p <port>:<port> " this tells docker which ports we need open for the image we are running. 
Without this we will not have access to our Jenkins image. In a browser we need to type in "localhost:8080"
![Screenshot 2025-02-12 at 2 12 34 PM](https://github.com/user-attachments/assets/5ebdcd79-bd18-416f-96e7-e83b239a03ee)

Perfect, we can now access our Jenkins instance. Back in docker click on the name of your container. 
This will take you to the logs and there you can navigate where it has your "username" and "password"
We are now logged in!

















