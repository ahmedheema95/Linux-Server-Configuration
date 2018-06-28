# Linux-Server-Configuration
### Project Details :
>as mentioned on Udacity web site "You will take a baseline installation of a Linux server and prepare it to host your web applications. You will secure your server from a number of attack vectors, install and configure a database server, and deploy one of your existing web applications onto it".
  
### Server Details :
* Public ip address : 54.213.197.180
* Application URL : http://ec2-54-213-197-180.us-west-2.compute.amazonaws.com/
* SSH Port : 2200

### Basic Configuration :
* The project is hosted on **Amazon Web Service (AWS) LightSail Ubuntu VM** 
* I used **Gitbash** on windows to connect to the VM instance through this steps :
    * Download the instance's private key by navigating to the Amazon Lightsail 'Account page'.
    * A file with extension .pem will be downloaded; open it.
    * Copy the text and put it in a file called light_key.rsa (or any other name to help you could recall anytime) in the local machine ~/.ssh/ directory
    * Run chmod 600 ~/.ssh/lightrail_key.rsa to give permissions to access the file
    * Run ssh -i ~/.ssh/light_key.rsa ubuntu@public-ip-address, to login to the VM

### Update Ubuntu Packages :
Run the following commands to update existing and installed packages
* `sudo apt-get update`
* `sudo apt-get upgrade`

### UFW Configuration (Uncompiled Firewall) :
1.    Change the SSH port from 22 to 2200 (open up the /etc/ssh/sshd_config) file :
      * `sudo nano /etc/ssh/sshd_config` 
2.    Find the line which contains port number and change number to 2200 , then restart SSH : 
      * `sudo service ssh restart`
3.    Set the UFW to block everything coming in : 
      * `sudo ufw default deny incoming`
4.    Set the UFW to allow everything outgoing :
      * `sudo ufw default allow outgoing`
5.    Set the UFW to allow SSH :
      * `sudo ufw allow ssh`
6.    Allow all tcp connections for port 2200 (SSH) :
      * `sudo ufw allow 2200/tcp`
7.    Set the UFW to allow a basic HTTP server
      * `sudo ufw allow www`
8.    Set the UFW to allow NTP (port 123) :
      * `sudo ufw allow 123/udp`
9.    Deny port 22 (as SSH will be configured to port 2200 instead)
      * `sudo ufw deny 22`
10.   Enable UFW
      * `sudo ufw enable`
11.   Then you can check the status of the UFW and open ports :
      * `sudo ufw status`
12.   Then we need to update the firewall configuration on the lightsail AWS instance and change the same ports as configured above
13.   You can now login through new terminal window :
      * `ssh -i ~/.ssh/lightrail_key.rsa -p 2200 ubuntu@public-ip-address`

### Create grader User :
1.    Add user :
      * `sudo adduser grader`
2.    After the user is added we need to give it sudo permissions :
      * `sudo visudo`
3.    Add the following line after to the ones that look same :grader ALL=(ALL:ALL) ALL
4.    Then we need to allow grader to login to VM :
5.    Open another Gitbash window on your local machine and run
6.    `ssh-keygen` and choose name for this key as grader_key
7.    Then from the VM instance navigate to the grader home directory
      * `cd /home/grader`
8.    Then create a directory '.ssh' and a file 'authorized_keys' inside it
      * `sudo mkdir .ssh`
      * `sudo touch authorized_keys`
9.    Then copy the content of the key generated on the local machine to the autorized_keys file on the VM
10.   Run `chmod 700 .ssh` on the VM
11.   Run `chmod 644 .ssh/authorized_keys` on the VM
12.   Run `chown -R grader:your_user .grader `on the VM
13.   Make sure key-based authentication is forced (log in as grader, open the /etc/ssh/sshd_config file, by running `sudo nano /etc/ssh/sshd_config file` and find the line that says, '# Change to no to disable tunnelled clear text passwords'; if the next line says, 'PasswordAuthentication yes', change the 'yes' to 'no'; save and exit the file; `run sudo service ssh restart`
14.   Then login to the VM machine through your local machine :
      * `ssh -i ~/.ssh/grader_key -p 2200 grader@public-ip-address`

### Deploy Item Catalog :
#### Configure the local timezone to UTC:
1.    Open the timezone selection dialog:
      * `sudo dpkg-reconfigure tzdata`
2.    Then chose 'None of the above', then UTC.
#### Install and configure Apache to serve a Python mod_wsgi application:
1.    Install Apache web server:
      * `sudo apt-get install apache2`
2.    Install mod_wsgi :
      * `sudo apt-get install python-setuptools libapache2-mod-wsgi`
3.    Restart apache server :
      * `sudo service apache2 restart`
#### Clone the app from GitHub:
1.    Install git:
      * `sudo apt-get install git`
2.    Navigate to /var/www and create 'catalog directory' and navigate to it
      * `cd /var/www`
      * `sudo mkdir catalog`
      * `cd catalog`
3.    Then clone the project from github repository :
      * `sudo git clone https://github.com/ahmedheema95/ItemCatalog.git clone`
####      
