# Ansible_Playbook_through_Jenkins

### In this tutorial, we are gonna learn how to run ansible playbook through Jenkins CI/CD pipeline. (Simple DevOps project for beginners).
<img src="https://user-images.githubusercontent.com/117680100/220602663-21b46cb0-7131-479f-8491-8647bf6becfd.png" width="60%" height="60%">

### What is Jenkins?
Jenkins is an open source continuous automation software DevOps tool written in the Java programming language. It helps automate the parts of software development related to building, testing, and deploying, facilitating continuous integration (CI) and continuous delivery (CD). It is a server-based system that runs in servlet containers such as Apache Tomcat.

### What is Ansible?
Ansible is an open source, command-line IT automation software application written in Python and developed by **Red Hat**. It can configure systems, deploy software, and orchestrate advanced workflows to support application deployment, system updates, and more. 
Let's say we have hundreds of servers, and we want add some files to those servers. Istead of log in every server and running some commands, we just create simple Ansible playbook where it goes to servers and add files at the same time which is more efficient in terms of time consuming.


### Creating new user
**STEP 1.** Before installation it is better to add new user and install Jenkins, Ansible and run commands with that user.

```
useradd jenkins
# make password for new user
passwd jenkins
```
- Make created user as part of sudoers which can easily run commands with privileges of root user.

```
vim /etc/sudoers/
# type following line under "root   ALL=(ALL)      ALL"
jenkins   ALL=(ALL)      NOPASSWD:  ALL
```

### Installing ansible in our machine where we will also install Jenkins.

- **STEP 2** Make sure that phyton is installed in your machine.
```
python3 -m pip -V
```
- If all is well, you should see something like the following: `pip 23.0.1 from /home/jenkins/.local/lib/python3.9/site-packages/pip (python 3.9)`.

- Use pip to install ansible package.
```
python3 -m pip install --user ansible
```
- Confirm that ansible is installed succesfully. by this command you check asible-core package that has been installed.
```
ansible --version
```
- To check the version of the ansible package that has been installed:
```
python3 -m pip show ansible
```


### Instaliing Jenkins in our machine where we also istalled Ansible before.

- **STEP 3** To install jenkins run below commands in your machine.

```
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
#If wget command is not installed in your machine, try to install it wit 'sudo yum install wget -y'
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo yum upgrade -y
# Add required dependencies for the jenkins package
sudo yum install java-11-openjdk
sudo yum install jenkins
sudo systemctl daemon-reload
```

- If Jenkins succesfully installed then start it.

```
sudo systemctl enable jenkins
sudo systemctl start jenkins
# make sure jenkins is started succesfully, active nad running by following command
sudo systemctl status jenkins
```

- As a default Jenkins running on port number `8080`, so if you are using firewall make sure that port is added to your firewall.

```
sudo firewall-cmd --zone=public --add-port=8080/tcp
# do not forget to reload
sudo firewall-cmd --reload
```
- **NOTE :** If you have application which is already running on port number `8080` then you need to forward Jenkins to work another port number. 

### Configuring Jenkins

- **STEP 4** Now, you can go to your browser and type `http://<ip address of your machine>:8080` , then you will see following screen below:

All you need to do is go to your machine terminal and run `cat <path-to-initial-password>` , then copy resulted password and paste it to the `Administrator password` part.

<img src="https://user-images.githubusercontent.com/117680100/220190303-4c276897-bd90-4ac0-a7e2-81a06e6252cc.png" width="60%" height="60%">

- After typing password you will see below screen. Click metioned section `Install suggested plugins` or you can also install specific plugins as you want.

<img src="https://user-images.githubusercontent.com/117680100/220194500-511e0f0d-a56b-478d-ab20-aa95ef565d6e.png" width="60%" height="60%">

- When installation was completed, below screen will seem. You have two options here. You can skip creating user (you can create it later) and continue as a admin or write down details of user and continue.

<img src="https://user-images.githubusercontent.com/117680100/220195942-c566f68f-0f4a-41cb-b9a7-53ea8ec39ece.png" width="60%" height="60%">

- In following window, `save and continue` for Jenkins url where you will able to access your Jenkins environment.

<img src="https://user-images.githubusercontent.com/117680100/220196490-011912ae-779f-4679-9455-556c83d2b65e.png" width="60%" height="60%">

- Now our Jenkins is ready, so press `Start using Jenkins` for going further steps.

<img src="https://user-images.githubusercontent.com/117680100/220196824-06ed912d-948e-42b4-a47e-eda84e7ec622.png" width="60%" height="60%">


### Adding Ansible plugins to Jenkins

- **STEP 5** On the left side of screen click the `Manage Jenkins`.

<img src="https://user-images.githubusercontent.com/117680100/220679175-7473cef1-6033-4236-9b2b-7811e1a28c71.png" width="60%" height="60%">

- In the opening window go to the next section which is `Manage Plugins`.

<img src="https://user-images.githubusercontent.com/117680100/220679175-7473cef1-6033-4236-9b2b-7811e1a28c71.png" width="60%" height="60%">

- After going to the Manage Plugins section, on the left side of the screen click on `avaliable pulgins` and just type ansible then install it.

<img src="https://user-images.githubusercontent.com/117680100/220685920-6d8a1278-04cd-431d-85ec-b14cc4b8586b.png" width="70%" height="70%">

- When installation was completed, go to the `global tool configuration` from `manage jenkins` section, then scroll down and add ansible. In name part you can give any name as you want , but for the `path to ansible executables directory` go to your machine where ansible was installed type `which ansible` command and copy it paste path to that section.

<img src="https://user-images.githubusercontent.com/117680100/220689448-c5000a94-7dbc-41d9-97f4-1327d50afa5d.png" width="60%" height="60%">

<img src="https://user-images.githubusercontent.com/117680100/220690413-f6de4e0d-e333-48e4-8671-cb8c20af1b8b.png" width="60%" height="60%">

**NOTE:** if you installed ansible with different user, your path to ansible command would not be the same. it will be something like `/home/username/.local/bin/ansible`.

### Creating Pipeline Scripts for Ansible and Git.

#### Pipeline script for git

- **STEP 6** In the main menu , click `New Item` and in the opened window type the name as you want to call it then click `Pipeline` and click `OK` .

<img src="https://user-images.githubusercontent.com/117680100/220692504-a7d4816b-fb14-4f6e-bfb2-bd4f814b3c93.png" width="70%" height="70%">

- Scroll down and in the pipeline section click `pipeline syntax` where it will helps us to generate pipeline for github repository and ansible.

<img src="https://user-images.githubusercontent.com/117680100/220695964-5731373c-01b3-4ce2-b773-1e8575882b6b.png" width="70%" height="70%">

- **STEP 7** As shown in the below picture: 
    - choose `git:Git` from `Sample Step`
    - Paste url of ansible playbook if you have already one. If you do not have any ansible playbook you can use my 
    `Prometheus-Node-Exporter_Ansible-Playbook` repository for sample.
    - Branch name of your repository
    - Click to `Generate Pipeline Script` (copy it and save it we will use it after few steps in main pipeline)

    <img src="https://user-images.githubusercontent.com/117680100/220698528-8af58515-6442-41a1-aeec-dbc2e70d0854.png" width="75%" height="75%">

    <img src="https://user-images.githubusercontent.com/117680100/220704308-03063e20-e3d2-431f-aab5-dfdc7f6bc1e0.png" width="70%" height="70%">
**SPECIAL NOTE:** If you see following error while pasting your github repository url, it means `GIT` is not installed in your machine. Therefore, go to your Linux machine and install git with `sudo yum isntall git -y` command.

  <img src="https://user-images.githubusercontent.com/117680100/220699381-4ed352a1-4b0f-454b-a7d8-0337512adf08.png" width="80%" height="80%">
  

#### Pipeline script for Ansible

- **STEP 8** Now, we need to generate pipeline for Ansible as well. 
    - Choose `asniblePlaybook: invoke an Ansible Playbook` from `Sample Step`.
    - For `Playbook file path in workspace` , write down name of your .yml file in the repository.
    - Type your inventory file path in repository to `Inventory file path in workspace.`
    <img src="https://user-images.githubusercontent.com/117680100/220705116-26d320d7-05dc-4f55-b47e-8d17837d63c3.png" width="75%" height="75%">
    - For `SSH connection credentials`: click to `+ Add` and `Jenkins` then enter details for SSH connection.

    <img src="https://user-images.githubusercontent.com/117680100/220709292-bc4b8754-60da-4f34-b9a8-aa784710ee86.png" width="75%" height="75%">
    
    - for the private key go to your Linux machine and copy private key and directly paste it to `Add` section

    <img src="https://user-images.githubusercontent.com/117680100/220709316-65135883-afdf-400a-b8e8-7b2fb832594d.png" width="75%" height="75%">
    After adding your private key then for `SSH connection credentials`, choose your private key which you added.

    - For `Become username` and `Sudo username` use  an username which we created while installing Jenkins and Ansible.
    - Click to `Generate Pipeline Script` and copy script (we will need at at main pipeline).
     <img src="https://user-images.githubusercontent.com/117680100/220712581-646684ee-ef25-4849-bbd1-e57312d495f3.png" width="75%" height="75%">

### Create Pipeline for building

- **STEP 9** Now, go back to the pipeline where we created new pipeline item in **Step 6** , and type below script to there also ypu need to paste your git and Ansible pipeline scripts thath we generated before in **Step 7** and **Step 8** to there. Then click save.

```
  pipeline{
    agnet any
    stages{
        stage{'SCM Checkout'){
            steps{
                <your github pipeline script>   
            }
        }
        stage('Execute Ansible'){
            steps{
                <your ansible pipeline script>
            }    
        }
    }
    
}
```

 <img src="https://user-images.githubusercontent.com/117680100/220714798-d530e1a4-9a74-49e4-a2cb-d15140e32604.png" width="85%" height="85%">

- **STEP 10** Now, go to the your pipeline and click `Build Now`, then it will start to build our pipeline. You check status of your pipeline building process as well as looking at logs on the right side. If everything is succesfully completed, then you can check your remote target server for your ansible playbook.

 <img src="https://user-images.githubusercontent.com/117680100/220718930-349df105-aa43-45c6-9f76-d9eb33d8f83c.png" width="80%" height="80%">

 ### Congrats, You have built your first Jenkins project. KEEP GOING!!! 

