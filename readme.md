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

---  

****📂 Terraform Infrastructure Setup****

First Let's create our repo.
![Screenshot 2025-02-12 at 1 23 47 PM](https://github.com/user-attachments/assets/743a08dd-6065-411e-9ed2-3cab624cd6da)

Now we need to copy the url of our repo in the "Code" drop down. Then we can go into VScode and remote into the repo from our local device. The command we need to run in our VSCode terminal is "git remote add origin <your_repos_url>"
![Screenshot 2025-02-12 at 1 35 26 PM](https://github.com/user-attachments/assets/99ba00b6-e8c1-45e6-ab25-5c264dda207a)

Our terraform files will now be uploaded to our GitHub repo. We need to run the following commands:

"git init" (To start git on our local folder)

"git add ." (This will add all the files in our current working directory. If you don't want to add all files there are two ways to resolve this. We could use a [.gitignore file](https://git-scm.com/docs/gitignore)

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


---

****Now we will set up plugins in Jenkins and AWS credentials in Jenkins****

Once in Jenkins go to dashboard>>Manage Jenkins>>Plugins>>Available plugins
This link will show you the plugins needed [it's a long list](https://docs.google.com/document/d/1gFvo-75iptPx6gZqBC5iXSSGteMEGhg8fnyC1cUZSws/edit?usp=sharing)
It may take some time to install these.

Now our plugin list should look something like this to start with.
![Screenshot 2025-02-12 at 2 31 48 PM](https://github.com/user-attachments/assets/00eacc7e-f5eb-4f55-b181-5df565e022f4)

Next lets go to our AWS console
![Screenshot 2025-02-12 at 2 33 56 PM](https://github.com/user-attachments/assets/cbae5f9a-58b6-405b-a2b9-cdaa3336d8b0)

On the top right it shows our user name. Click the drop down arrow then "Security credentials" 
Navigate down to "Access Keys" click "create new" and copy your "access key id" and "secret id" we need these for Jenkins.

Now back in Jenkins navigate to Dashboard>>Manage Jenkins>>Credentials at the bottom of the screen "Stores scoped to Jenkins"
![Screenshot 2025-02-12 at 2 40 40 PM](https://github.com/user-attachments/assets/52176255-6ff6-42b5-be85-29fe6aa0f42e)

Click "+ Add credentials" on the right hand side.
![Screenshot 2025-02-12 at 2 41 34 PM](https://github.com/user-attachments/assets/c63a8193-82ce-45fd-9d9f-a4ed3abf4743)

Under the "Kind" drop down select "AWS Credentials"
![Screenshot 2025-02-12 at 2 42 20 PM](https://github.com/user-attachments/assets/3d2015f8-47e1-459b-9517-e5addff3ee1a)

ID: A name of your choosing. (Remember this name EXACTLY how you spelled it)

Description: Any description

Access Key ID: This HAS to be the "Access Key ID" we got from AWS

Secret Access Key: This HAS to be the "Secret Access Key" we got from AWS

Click create.

No we need to install and move terraform from the terminal:

In terminal/gitbash/docker-terminal run the command "docker exec -it <your_container_name> bash"

Now let's install the awscli "apt update && apt install -y awscli"

Next we need to update our packages "apt update && apt install -y curl unzip"

Next make a directory for our terraform files "mkdir -p /home/jenkins/bin"

Now we need to download the terraform Binary. Mind you this command is for Apple Silicon(ARM64) Chipset 
"wget https://releases.hashicorp.com/terraform/1.10.5/terraform_1.10.5_linux_arm64.zip"

**For x86 you will need a different binary.** 

Now we need to extract and Move Terraform to the correct location with this command:
"unzip terraform_1.10.5_linux_arm64.zip
mv terraform /usr/local/bin/
chmod +x /usr/local/bin/terraform"

Next lets check if terraform is working with "terraform -version"
We should see an output some like this "Terraform v1.10.5
on linux_arm64"

---

****Now we will setup the pipeline and deploy to AWS!****

Now back in our browser we need to navigate to the "dashboard" and click "new item"
Enter a name and select "Pipeline"
Enter your description of your choice
![Screenshot 2025-02-12 at 3 31 16 PM](https://github.com/user-attachments/assets/71c0c47f-360a-406a-adcd-2489d3e64864)

Scroll down to "Pipeline" under the "Definition" drop down select "Pipeline script from SCM">>Under the "SCM" drop down select "Git"
Now under Repositories>>URL enter the URL of the repo we created earlier!
Under "Branches to Build">>"Branch Specifier" add the name of the branch you pushed to in Github
![Screenshot 2025-02-12 at 3 35 47 PM](https://github.com/user-attachments/assets/9ad7cc2e-13cf-46da-a34f-ac387c30ba6b)

Make sure "Script Path is 'Jenkinsfile'" Then click save.

Now let's deploy
![Screenshot 2025-02-12 at 4 04 05 PM](https://github.com/user-attachments/assets/bfb99392-58b3-4766-a2d2-82328b15d939)

Looks good now let's scroll to the bottom and click "Deploy"

![Screenshot 2025-02-12 at 4 04 44 PM](https://github.com/user-attachments/assets/65e0de39-3f99-495f-ae94-724dd1af4ce5)

Alright our Images are now up and running in AWS!
![Screenshot 2025-02-12 at 4 09 54 PM](https://github.com/user-attachments/assets/5d19bcb1-12e2-4688-9ba0-de7ae8825fcb)










