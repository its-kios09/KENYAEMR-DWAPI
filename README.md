# KENYAEMR-DWAPI
Official images for running DWAPI on 64 bit linux systems 

#### 1.0 Download and install Docker
If you do not have Docker installed, visit https://docs.docker.com/get-started/#download-and-install-docker and choose your preferred operating system to download and install Docker

#### 1.1 Install Docker in UBUNTU 20.0.4
If you do not have Docker installed, use the following steps:

#### Step 1.1.0: Update the software repository
    sudo apt update
#### Step 1.1.1: Download Dependencies
    sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

The above-mentioned command:
  - Gives the package manager permission to transfer files and data over https.
  - Allows the system to check security certificates.
  - Installs curl, a a tool for transferring data.
  - Adds scripts for managing software.
 
#### Step 1.1.2: Add Docker's GPG Key
Next, add the GPG key to ensure the authenticity of the software package.
      
      curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
#### Step 1.1.3: Installing the Docker Repository
Now install the Docker repository using the command:

      sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu  $(lsb_release -cs)  stable"
  
The command installs the latest repository for your specific Ubuntu release (in this case, 20.04).

#### Step 1.1.4: Installing the Latest Docker
Start by updating the repository again
      
      sudo apt update
      
Now you can install the latest Docker version with:
      
      sudo apt-get install docker-ce

#### Step 1.1.6: Verifying Docker Installation:
To confirm the installation check the version of Docker:

      docker --version

#### Step 1.1.7: Enable Docker Service:
To start the Docker service run the following command:
      
      sudo systemctl start docker

#### Step 1.1.8: Enable Docker to run at startup with:

      sudo systemctl enable docker
      
#### Step 1.1.9: To check the status of the service, use the command:

      sudo systemctl status docker

### DWAPI Setup

#### Step 1.2.0: New Installation:

Install DWAPI
    
    sudo docker run --name dwapi -p 5757:5757 -p 5753:5753 -d --restart unless-stopped kenyahmis/dwapi:latest

#### Step 1.2.1: Upgrading Existing Installation:

Upgrading DWAPI to latest version

    sudo docker ps -a | grep "dwapi" | awk '{print $1}' | xargs sudo docker rm -f
    sudo docker images -a | grep "dwapi" | awk '{print $3}' | xargs sudo docker rmi
    sudo docker run --name dwapi -p 5757:5757 -p 5753:5753 -d --restart unless-stopped kenyahmis/dwapi:latest

### MySQL Setup

Configure MySQL to allow remote access. Edit your my.cnf file which is found on /etc/mysql/my.cnf OR /etc/mysql/mysql.conf.d/mysqld.cnf depending on your mysql installation

#### 1.3.0: MySQL 5.5
Change line 
      
      bind-address = 127.0.0.1 to #bind-address = 127.0.0.1

#### 1.3.1: MySQL 5.6 - add the line if it does not exists
	    
      bind-address = *
      
#### 1.3.2: Create a DWAPI database user for MySQL

      create user 'dwapi'@'%' identified by 'dwapi';
      
#### 1.3.3: Assign privileges to the DWAPI database user for MySQL

      GRANT ALL PRIVILEGES ON *.* TO 'dwapi'@'%' IDENTIFIED BY 'dwapi' WITH GRANT OPTION; 
      FLUSH PRIVILEGES;

### Using DWAPI
#### 1.4.0: Start DWAPI
On your browser open dwapi on https://localhost:5753

Configure your data sources and verify registries
Please note that for the database connection will need to specify the IP address of the computer and NOT localhost or 127.0.0.1

#### 1.4.1 Restart DWAPI:
    sudo docker restart dwapi

#### 1.4.2 Troubleshooting DWAPI:
View log files

      sudo docker exec -it dwapi ls logs
      
Copying log files folder to your pc current directory.

      sudo docker cp dwapi:/app/logs/ .


