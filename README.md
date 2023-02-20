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

![unlock-jenkins](https://user-images.githubusercontent.com/117680100/220190303-4c276897-bd90-4ac0-a7e2-81a06e6252cc.png)

<img src="https://user-images.githubusercontent.com/117680100/220190303-4c276897-bd90-4ac0-a7e2-81a06e6252cc.png" width="75%" height="75%">

All you need to do is go to your machine terminal and run `cat <path-to-initial-password>` , then copy resulted password and paste it to the `Administrator password` part.

- After typing password you will see below screen

![Customize_jenkins](https://user-images.githubusercontent.com/117680100/220194500-511e0f0d-a56b-478d-ab20-aa95ef565d6e.png)
Click metioned section `Install suggested plugins` or you can also install specific plugins as you want.





