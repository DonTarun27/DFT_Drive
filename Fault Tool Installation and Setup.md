# Installation
In order to use Fault, you have three options
1. Using the Docker Image (Windows, macOS, Linux & WSL)
2. Using Nix (macOS, Linux)
3. Bring-your-own-dependencies (macOS, Linux)
## 1. Using the Docker Image
Docker is a software working at the OS level that allows small environments called “containers” to be run at-will.
It works on Windows, macOS, Linux and WSL (Windows Subsystem for Linux), where for the first two, a Linux virtual machine is used.
For instructions on how to install Docker Engine on Linux, check Docker’s website - https://docs.docker.com/engine/install/
### Getting the Fault Docker image
1. After installing Docker, run the below command in your terminal of choice (I am using **WSL - Ubuntu 24.04.2 LTS**):
```
docker pull ghcr.io/aucohl/fault:0.9.4
```
2. You can then run Fault commands using that image. For example, to run fault --version:
```
docker run -ti --rm ghcr.io/aucohl/fault:0.9.4 fault --version
```
3. It should print the version name as **0.9.4**, if you see that, you have successfully set the Fault environment up on your machine.
4. To use the current directory inside the Docker container, you need to add these options to the command:  
-v </path/to/directory>:</path/to/directory> -w </path/to/directory>  
Obviously, you should replace </path/to/directory> with your current directory.  
**[!TIP]** You can add as many -v's as you want to mount multiple directories.
5. This makes the final command as:
```
docker run -ti -v </path/to/directory>:</path/to/directory> -w </path/to/directory> --rm ghcr.io/aucohl/fault:0.9.4 fault --version
```
# Steps to run Docker without using sudo
### 1. Create the Docker Group:
During the installation of Docker, a group named **docker** is usually created. You can check if it exists by running the below command:
```
sudo groupadd docker
```
If the group already exists, you will see an error message, which you can ignore.
### 2. Add Your User to the Docker Group:
Run the below command to add your user to the Docker group, replace $USER with your username if necessary:
```
sudo usermod -aG docker $USER
```
This command appends your user to the Docker group, granting permission to communicate with the Docker daemon.
### 3. Log Out and Log Back In:
For the changes to take effect, you need to log out of your session and log back in. This refreshes your group memberships.
### 4. Verify Docker Access:
After logging back in, you can test if you can run Docker commands without sudo by running the below command:
```
docker run hello-world
```
If the command runs successfully and you see a confirmation message, you have configured Docker correctly.
# Docker Container Setup of Fault Tool Working Directory
1. Change your path to a directory you prefer in your user home directory (\~) like I changed it to **~/Documents**
2. Clone the Fault Tool GitHub repository which contains tech files & examples to your current directory by running the below command:
```
git clone https://github.com/AUCOHL/Fault
```
So that it creates a directory named Fault in your current directory like in my case it is **~/Documents/Fault**

3. Now, to setup & use the current directory inside the Docker container, run the below command:
```
vi ~/.bashrc
```
And then insert the below line at the end of the file:
```
alias fault='docker run -u 1000:1000 -ti -v /home/$USER/Documents/Fault:/home/$USER/Documents/Fault -w /home/$USER/Documents/Fault --rm ghcr.io/aucohl/fault:0.9.4 fault'
```
4. Log Out & Log Back In or run the below command in the current terminal:
```
source ~/.bashrc
```
Now you can use **fault** as a command instead of typing the lengthy Docker command every time.

5. Check whether running the below command prints the version name as **0.9.4**:
```
fault --version
```
6. If the Docker is not running or inactive, run the below commands:
```
sudo systemctl stop docker
sudo rm /var/lib/docker/volumes/metadata.db
sudo systemctl start docker
sudo systemctl status docker
```
7. If it's still not running or being inactive, run the below commands:
```
ps axf | grep docker | grep -v grep | awk '{print "kill -9" $1}' | sudo sh
sudo systemctl start docker
sudo systemctl status docker
```
**Note:** I filtered and simplified the steps given in this original Fault Open Source Tool website - https://fault.readthedocs.io/en/latest/installation.html and pulled the latest version from the GitHub repository - https://github.com/aucohl/Fault/pkgs/container/fault
