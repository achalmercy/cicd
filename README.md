# aws-cicd-project
# Deployment of end-to-end application in node.js using Jenkins CICD with GitHub Integration

---

| &nbsp; | &nbsp; |
| --- | ----------- |
| **Objective** | - Deploy end-to-end application in node.js using Jenkins CICD with GitHub Integration <br>- Trigger Jenkins pipeline automatically once the code is pushed on GitHub |
| **Approach** | - Using Amazon's Elastic Compute Cloud (EC2) for running application on the Amazon Web Services (AWS) infrastructure <br>- Containerize application by creating Dockerfile<br>- Integrate GitHub with Jenkins using Webhook |
| **Impact** | - Jenkins pipeline triggers automatically once the code is pushed on GitHub</br>- Accomplish faster quality releases by automating CI/CD pipelines |
<br>

**Primary Technology:** Github, Docker, Jenkins, aws EC2 service
<br><br>

![Design Thinking Whiteboard in Yellow Blue Basic Style](https://user-images.githubusercontent.com/102405945/211872655-4ed92a9f-bf03-41bc-ac92-043594863cd8.png)


<br><br>
---

## 1. Creating a Node App
Before we write any CI/CD pipeline we need an application to test and deploy. We are going to build a simple **to-do application** in node.js. Then, create new repository under your GitHub account and name it “node-todo-cicd”.

![Screenshot 2024-12-12 095319](https://github.com/user-attachments/assets/9e3204d5-90cb-4644-9832-9ae86c6de0b6)


## 2. Creating AWS EC2 instance

### 2.1 Creating AWS EC2 instance
After logging into your AWS account, search for EC2
![1c](https://github.com/user-attachments/assets/40b4c88d-5bd7-45d5-93f3-ec48c7f2c4c5)

Select **Instances(running)**
![Iniyavan](https://github.com/user-attachments/assets/646840ed-b163-4d8e-b437-ddbd5a329c3a)

Click **Launch instances**
![Iniyavan1](https://github.com/user-attachments/assets/fee7a20e-0e96-4579-8425-fbdf3dc7d05c)
Enter Name and select **Ubuntu**
![Iniyavan3](https://github.com/user-attachments/assets/097b9b68-5308-4f3f-9d09-12be4c5b61d1)

Select **t2.micro** as Instance type and create new key pair to connect to the server
![Iniyavan4](https://github.com/user-attachments/assets/3bff7792-fc72-4ee5-8b14-24adb2adb0a7)


Enter **key pair name** and select **RSA** as Key pair type and **.pem** as Private key file format. Then, click **Create key pair**
![Iniyavan5](https://github.com/user-attachments/assets/af3cd032-1429-4c92-8e0f-7cf5dc7ab1eb)


Finally, click **Launch instance**
![Iniyavan6](https://github.com/user-attachments/assets/a7ef0534-d77e-41cb-b21e-941cf03391eb)




### 2.2 Connecting AWS EC2 instance
Click **Connect** on the top of the screen
![Iniyavan7](https://github.com/user-attachments/assets/76254530-f4cf-415f-8f50-f7a56c777c5d)

Click **Connect**
![Iniyavan8](https://github.com/user-attachments/assets/2f018ef3-8f14-4ef1-91f7-924e6d0ec2d8)


## 3. Installing Jenkins on AWS

### 3.1 Installing Java
Install Java using following commands: <br>
**sudo apt update**<br>
**sudo apt install openjdk-22-jre**<br>
**java -version**<br>
![Iniyavan9](https://github.com/user-attachments/assets/98596647-a058-4c73-bc3b-4c2c3d0097ae)
![Iniyavan10](https://github.com/user-attachments/assets/444bbf77-ce4c-476a-a316-f764c489ffab)

### 3.2 Installing Jenkins
Install Jenkins using following commands: <br>
**curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee \   /usr/share/keyrings/jenkins-keyring.asc > /dev/null **<br>
**echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \   https://pkg.jenkins.io/debian binary/ | sudo tee \   /etc/apt/sources.list.d/jenkins.list > /dev/null**<br>
**sudo apt-get update**<br>
**sudo apt-get install jenkins**<br>
![Iniyavan11](https://github.com/user-attachments/assets/0d8e1504-19c5-45de-af1c-e2ac488ed9d1)


### 3.3 Start Jenkins
Start jenkins using following commands: <br>
**sudo systemctl enable jenkins**<br>
**sudo systemctl start jenkins**<br>
**sudo systemctl status jenkins**<br>
![Iniyavan12](https://github.com/user-attachments/assets/58c210bd-e29d-434c-a63a-2576d338e9b0)


### 3.4 Open port 8080 from AWS Console
Go to Instances. Click on Security tab
![Iniyavan13](https://github.com/user-attachments/assets/a39ba153-953a-4ae0-96a2-640182a0dd25)

Click on **Add rule** and add port 8080 and select **My IP** and then click **Save rules**
![Iniyavan14](https://github.com/user-attachments/assets/67f5f007-8678-4cd3-ac60-a62e7f7861b0)

### 3.5 Unlock Jenkins
Open port 8080 on new tab
![Screenshot 2024-12-12 122628](https://github.com/user-attachments/assets/697e171e-093b-49f1-ad55-3182aaffcdde)



Paste the password in Administrator password to unlock jenkins
![25](https://user-images.githubusercontent.com/102405945/211763367-b2d215be-ea2b-4a45-bb20-2ec3acef41fe.png)

Install suggested plugins
![26](https://user-images.githubusercontent.com/102405945/211763394-0ea3d5af-8c15-466d-8be2-99487cc1d873.png)
![27](https://user-images.githubusercontent.com/102405945/211763441-25d3a088-6119-43b0-a04e-cde33cfaca52.png)

Create First Admin User
![28](https://user-images.githubusercontent.com/102405945/211763485-8449f84f-313a-4478-ab81-bebdf36a808f.png)

Enter Jenkins URL and click **Save and Finish**
![29](https://user-images.githubusercontent.com/102405945/211763529-e7ac31bd-38b6-45ef-ab43-118ff9b93fa3.png)

Finally, Jenkins is ready. Click on the button **Start using Jenkins**
![30](https://user-images.githubusercontent.com/102405945/211763586-17fdc134-9757-4b82-ad67-5eb57fc4ab9c.png)


### 3.5 Connect GitHub and Jenkins
Create new job by clicking **New item**
![32](https://user-images.githubusercontent.com/102405945/211767102-7ba3199c-a108-4a4f-8ebc-4ba25ef1af38.png)

Enter an item name. Select **Freestyle project**. Add description. Select **GitHub project** and add project url
![33](https://user-images.githubusercontent.com/102405945/211767157-ac02438b-86a8-4f66-8056-87be60df0588.png)

Select **Git** as Code Management and add **Repository URL**. Click on **Add** to add key
![34](https://user-images.githubusercontent.com/102405945/211767199-de9358d1-b081-4f8e-9da8-bead2ff93db3.png)

Generate SSH key on console using following commands: <br>
**ssh-keygen** <br>
**cd .ssh** <br>
**cat id_rsa_pub** <br>
Copy the public key
![Iniyavan15](https://github.com/user-attachments/assets/16a0f750-3779-4063-be6d-1277689b4241)
![Iniyavan16](https://github.com/user-attachments/assets/98b532ae-afd2-4957-aeb5-73f99a6add09)
![Iniyavan17](https://github.com/user-attachments/assets/9b522504-fb2f-4bea-ba7e-e8348e25ab45)


Click on **SSH and GPS keys** on the left pane and click on **Add SSH key**
![Screenshot 2024-12-12 115617](https://github.com/user-attachments/assets/714289ed-38d2-4def-a18b-0829254736d3)
![Screenshot 2024-12-12 115650](https://github.com/user-attachments/assets/44d1326f-76d1-41a7-97a1-f8f291ff6801)

Go to Jenkins, and select **SSH Username with private key** in kind
![41](https://user-images.githubusercontent.com/102405945/211767451-530ca1d9-1ea2-49c9-9637-c82a6a415357.png)
On console, enter the command **cat id_rsa** and copy the private key
![Iniyavan18](https://github.com/user-attachments/assets/c9900b82-356f-4409-97d9-718e286666a7)
Paste the private key in jenkins wizard
![43](https://user-images.githubusercontent.com/102405945/211767525-854c8421-a07e-4c04-b235-45be202d1247.png)

Select **ubuntu(This is for github and jenkins integration)** in credentials
![44](https://user-images.githubusercontent.com/102405945/211767548-5aac0cd6-ca4a-46e6-9d46-b7fd71f23dad.png)

Enter ***/master** in Branch Specifier and click **Save**
![45](https://user-images.githubusercontent.com/102405945/211767592-a9aaf324-878b-4716-a2f6-0de181d67b78.png)


### 3.6 Run node.js application
On console, run the following commands: <br>
**sudo apt install nodejs** <br>
**sudo apt install npm** <br>
**npm install** <br>
**node app.js** <br>
![56](https://user-images.githubusercontent.com/102405945/211807470-1bd37f3f-1e7e-4234-aa6a-f3b24fce6fd2.png)






## 4. Automating using Jenkins (CICD pipelines)

### 4.1 Automating commands using Jenkins (Execute shell)
On console, enter the following commands to terminate a container: <br>
**docker ps** <br>
**docker kill <containerid>** <br>


Click on **configure** on the left pane
![Screenshot 2024-12-12 122108](https://github.com/user-attachments/assets/1117550c-ee59-42b0-8ff8-99601056c9d5)

Click on **Build Steps** on the left pane and then select **Execute shell** in the Build Steps section
![73](https://user-images.githubusercontent.com/102405945/211852724-4f2d183b-7b4d-4f2e-a6a0-2a5b24f0bfe2.png)

Enter the following commands in the Execute shell to be executed: <br>
**docker build . -t node-app-todo** <br>
**docker run -d --name node-app-container -p 8000:8000 node-app-todo** <br>
Click **Save**
![74](https://user-images.githubusercontent.com/102405945/211852763-e95b9072-7510-4253-8c0b-8df43c5228c3.png)

Click on **Build Now** on the left pane
![75](https://user-images.githubusercontent.com/102405945/211852806-f72ff44c-e13a-47b9-b666-78492c92bc28.png)
![76](https://user-images.githubusercontent.com/102405945/211852837-9de83468-01ec-40b6-859f-43f9585e94f6.png)








### 4.2 Configure Jenkins
Go to Jenkins Job and click on **configure** on left pane


Click on **Build Triggers** on left pane and enable **GitHub hook trigger for GITScm polling. Finally, click **Save**
![95](https://user-images.githubusercontent.com/102405945/211859617-d48b1e56-8bb8-4b29-9c69-597ca9d9b397.png)

## Testing


Clearly, Jenkins pipeline are triggered as the code is pushed on GitHub
![97](https://user-images.githubusercontent.com/102405945/211859669-6eb83c45-2a41-4e63-9c51-db3e8fcabeb7.png)
![98](https://user-images.githubusercontent.com/102405945/211859697-c65ae885-146c-4e9b-86fa-4d52b570837e.png)
<br> <br>
![99](https://user-images.githubusercontent.com/102405945/211859754-cbbf68de-503f-4d5c-b1b7-c543218e07e7.png)



# cicd
