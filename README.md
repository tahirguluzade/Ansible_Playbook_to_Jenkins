# Ansible_Playbook_to_Jenkins

- **In this tutorial, we are gonna learn how to run ansible playbook through Jenkins CI/CD pipeline.**

**1.** First of all, we need to install Jenkins in our machine where we will run Jenkins.However before installation it is better to add new user and install jenkins and run with that command.

- Creating new user

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

- To install jenkins run below commands in your machine.

```
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
#If wget command is not installed in your machine, try to install it wit 'yum install wget -y'
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

- Now, you can go to your browser and type `http://<ip address of your machine>:8080` , then you will see following screen below:
All you need to do is go to your machine terminal and run `cat <path-to-initial-password>` , then copy resulted password and paste it to the `Administrator password` part.

<img src="https://user-images.githubusercontent.com/117680100/220190303-4c276897-bd90-4ac0-a7e2-81a06e6252cc.png" width="60%" height="60%">

- After typing password you will see below screen. Click metioned section `Install suggested plugins` or you can also install specific plugins as you want.

<img src="https://user-images.githubusercontent.com/117680100/220194500-511e0f0d-a56b-478d-ab20-aa95ef565d6e.png" width="60%" height="60%">

- When installation was completed, below screen will be seem. You have two options here. You can skip creating user (you can create it later) and continue as a admin or write down details of user and continue.

<img src="https://user-images.githubusercontent.com/117680100/220195942-c566f68f-0f4a-41cb-b9a7-53ea8ec39ece.png" width="60%" height="60%">

- In following window, `save and continue` for Jenkins url where you will able to access your Jenkins environment.

<img src="https://user-images.githubusercontent.com/117680100/220196490-011912ae-779f-4679-9455-556c83d2b65e.png" width="60%" height="60%">

- Now our Jenkins is ready, so press `Start using Jenkins` for going further steps.

<img src="https://user-images.githubusercontent.com/117680100/220196824-06ed912d-948e-42b4-a47e-eda84e7ec622.png" width="60%" height="60%">







