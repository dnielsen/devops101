## Also see
- [DevOps Workshop Community](https://groups.google.com/forum/#!forum/devopsworkshop)
- [Intro to DevOps app deployment 
 Tue Oct 17 2017 6:30-8:30PM Hacker Dojo](https://github.com/erich13/intro/wiki)
 - [Console](http://console.aws.amazon.com) 

### 0. Introduction

This document provides detailed, step-by-step instructions on creating a new AWS EC2 instance and deploying a provided Symfony2 application on it using LAMP (Linux, Apache, MySQL, PHP) stack. The application also uses a separate Redis database for certain parts of it, but the connection to Redis is not included in these instructions. These instructions are detailed for use with Mac OS; however, once logged onto the virtual machine provided by your EC2, the instructions are universal. The provided application is a website call “Spoutlet” and it will be cloned from an online github repository.



### 1.0 Sign up for AWS Free Tier 

- Sign up for Free Cloud Services – AWS Free Tier [https://aws.amazon.com/free/](https://aws.amazon.com/free/) 
    - “AWS Free Tier includes offers that expire 12 months following sign up and others that never expire.” 

### 1.1 Create AWS EC2 instance

- a) Log into [AWS console ](http://console.aws.amazon.com/ec2)and select “Services” &gt; “EC2” 
- b) Click on “Instances” 

 

### 2. Launch Instance

- a) Click on “Launch Instance” 
- b) Select “Ubuntu Server 16.04...” 
- c) Select the t2.medium size instance - VERY IMPORTANT 
    - click Next until you reach “Step 6: Configure Security Groups” 
    - Or Click on the “Step 6: Configure Security Groups” tab, located along the top of the screen. 

- d) Change “security group name” to “spoutlet - sample deployment” 
- e) Click “Add Rule” 
    - Add “Custom TCP Rule” with Port Range = 8000 
    - Change “Source” drop down menu to “Anywhere” 
    - Add “Custom TCP Rule” with Port Range = 0 
    - Change “Source” drop down menu to “Anywhere” 

- f) Click “Add Rule” 
    - Add “HTTP” 
    - Change “Source” drop down menu to “Anywhere” 

- g) Click “Review and Launch”, then review settings and click “Launch” 
- h) A pop up window will appear asking about a security key 
    - Select “Create a new key pair” from the first drop down box 
    - Enter “spoutlet_sampleEC2” for the key pair name, then “Download Key Pair” 
    - *IMPORTANT*: after downloading the key, place it in a safe and accessible place (perhaps create a backup copy and store it in the cloud); if lost, your key cannot be replaced, as AWS does not store your key after its creation 

- i) After storing your key in a safe and secure location, click “Launch Instances” 

### 3. Connect to Instance from AWS console

- a) After launching instance, return to running instances by clicking on “Services” &gt; “EC2” 
- b) Click on “Instances” 
    - You should see your deployed instance with “Initializing” status under “Status Checks” 
    - Rename the instance to “Spoutlet Sample” 

- c) Select your instance and click “Connect”, this should launch a pop up window with instructions 

### 4. Connect to Instance from Terminal

- With Terminal  / MacOS, From Termnal:  
    - chmod 400 spoutlet.publickey 
    - ssh -i "spoutlet.pem"[ubuntu@ec2-54-193-37-203.us-west-1.compute.amazonaws.com](mailto:ubuntu@ec2-54-193-37-203.us-west-1.compute.amazonaws.com) 

- With [PuTTY ](http://www.putty.org/)/ Windows  
    - Connecting to Your Linux Instance from Windows Using PuTTY[https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) 
    - Convert the .pem file to a .ppk file 


#### [Converting Your Private Key Using PuTTYgen](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)         

PuTTY does not natively support the private key format (.pem) generated by                 Amazon EC2. PuTTY has a tool named PuTTYgen, which can convert keys to the required                 PuTTY format (.ppk). You must convert your private key into this format (.ppk)                 before attempting to connect to your instance using PuTTY.

            

To convert your key from Public to Private (ppk to pem)

1. Start PuTTYgen (for example, from the Start menu,  choose All Programs &gt; PuTTY &gt; PuTTYgen).             
1. Under Type of key to generate, choose RSA.                     
1. If you're using an older version of PuTTYgen, choose SSH-2 RSA.                     
1. Choose Load. By default, PuTTYgen displays only files with the extension .ppk. To locate your .pem file, select the option to display files of all types.                     

                                                                                                                        

1. Select your .pem file for the key pair that you specified when you launch your instance, and then choose                             Open. Choose OK to dismiss the confirmation dialog box.                     
2. Choose Save private key to save the key in the format that PuTTY can use. PuTTYgen displays a warning about saving the key  without a passphrase. Choose Yes.                     
    - Note: 
    - A passphrase on a private key is an extra layer of protection, so even if your private key is discovered, it can't be used without the passphrase. The downside to using a passphrase is that it makes automation harder because human intervention is needed to log on to an instance, or copy files to an instance                     

3. Specify the same name for the key that you used for the key pair (for example, my-key-pair). PuTTY automatically adds the     .ppk file extension. 
4. Your private key is now in the correct format for use with PuTTY. You can now connect to your instance using PuTTY's SSH client. 

 

- a) Open a terminal window and navigate into the directory where you placed your “spoutlet_sampleEC2.pem” key (downloaded in step 2h) 
- b) Add privacy restrictions to your key by typing “chmod 400 spoutlet_sampleEC2.pem” on command line and hit enter 
- c) Launch virtual machine via ssh by typing “ssh -i "spoutlet_sampleEC2.pem" ubuntu@*your-instance*.compute.amazonaws.com” on command line and hit enter  
    - You can copy the above line under “example” from the “Connect to your instance” pop up window on your aws console and paste it in your terminal 

- d) You will be asked if you are sure you want to continue, type “yes” and hit enter 
- e) You should now be connected to your EC2 instance via a Virtual Machine in your terminal window (your username in the terminal window should now be “ubuntu@ip-*your-ip*”)  

### 4.5 Install php and composer

- Install composer by pasting the following line into your terminal and pressing enter: 

    - sudo apt-get update 
    - sudo apt-get install php  
    - sudo curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer 


### 5. Install Apache, MySQL, PHP on your Linux instance

- a) Install Apache 
    - Type “sudo apt-get update” and hit enter 
    - Type “sudo apt-get install apache2” and hit enter 
        - Type “y” and hit enter when you are asked if you would like to continue 

    - To make sure everything is working, type “apache2ctl configtest” on the command line and hit enter 
        - You should get an output of “Syntax OK” 

    - Check that your firewall allows web traffic from HTTP and HTTPS 
        - Type  sudo ufw app info "Apache Full" and hit enter 
        - Your output should show 80 and 443 under “Ports:” at the bottom of output 

- b) Install MySQL 
    - Type “sudo apt-get install mysql-server” 
        - Type “y” and hit enter when asked if you would like to continue 
        - You will be prompted to create a password for the MySQL root user, choose a password that you will remember and hit enter, then repeat it and hit enter 

    - Run secure install to improve security and other settings 
        - Type “sudo mysql_secure_installation” 
        - Enter the password for “root” you chose earlier 
        - For “VALIDATE PASSWORD PLUGIN”: type n and press enter 
        - “Change the password for root?” type n and press enter 
        - “Remove anonymous users?” type y and press enter 
        - “Disallow root login remotely?” type n and press enter 
        - “Remove test database and access to it?” type n and press enter 
        - “Reload privilege tables now?” type y and press enter 

- c) Install PHP 
    - Type “sudo apt-get install php libapache2-mod-php php-mcrypt php-mysql” and hit enter 

- d) Restart apache by typing “sudo systemctl restart apache2” and hit enter 
- e) Test that you have successfully installed LAMP 
    - Copy your instance’s “Public DNS” from the aws console (should look something like “ec2-54-183-158-228.us-west-1.compute.amazonaws.com”) 
    - Paste it into a web browser URL and hit enter, you should be directed to and Apache2 Ubuntu Default Page and prints the message “It works!”, followed by other configuration information 

- f) Navigate to “/var/www/html” and replace “index.html” with a new file named “index.php” and paste in this code: 
  

```<!DOCTYPE html>
<html>
<head>
        <title>Index</title>
</head>
<body>
        <h1>Index</h1>
</body>
</html>

<?php

echo "Index Page for Apache Server";

?>
```

### 6. Install phpMyAdmin for MySQL GUI

- a) Type “sudo apt-get update” on command line and hit enter 
- b) Type “sudo apt-get install phpmyadmin” and hit enter 
    - Type y and press enter when asked if you would like to continue 
    - When prompted to choose a web server for reconfiguration, choose apache2 and hit enter 
    - When asked whether you would like configure database for phpmyadmin with dbconfig-common, select yes and hit enter 
    - Create a password for phpmyadmin (Use the same one as your mysql password for root user) and hit enter, then confirm password 

- c) Allow for apache access to phpmyadmin 
    - Type “sudo vim /etc/apache2/apache2.conf” (or you can use a different text editor if you would like)  and hit enter 
    - At the end of the file, create a new line and type “Include /etc/phpmyadmin/apache.conf” 
    - Save and exit 

- d) Restart apache by typing “sudo systemctl restart apache2” 
- e) Test that all is working by returning to the browser page where you tested Apache connection (step 5e), and adding “/phpmyadmin” to the end of the URL.    For example: [http://ec2-13-56-200-209.us-west-1.compute.amazonaws.com/phpmyadmin](http://ec2-13-56-200-209.us-west-1.compute.amazonaws.com/phpmyadmin) 
    - You should be redirected to a login page where you can login with username “root” and the password you created in step 6b, then login and you will be able to view a GUI of your MySQL Databases 

 

### 7. Import MySQL Database into phpmyadmin

- a) On your phpmyadmin GUI, create a new database and and name it “Spoutlet” 
- b) Download   (ftp or with github) the file “spoutlet2.sql” from this drive folder... 

[https://drive.google.com/drive/folders/0B1haVZnrKolXTktkREZGeFZtY00?usp=sharing](https://drive.google.com/drive/folders/0B1haVZnrKolXTktkREZGeFZtY00?usp=sharing)

- c) Return to your phpmyadmin page and open the “Spoutlet” database 
- d) Click “import”, and then “choose file” 
    - Select the “spoutlet2.sql” file you downloaded and then finish the import by scrolling down and clicking “Go” 
    - Results: Import has been successfully finished, 33 queries executed. (spoutlet2.sql) 

- e) Your “Spoutlet” database should now be populated with 6 tables (“comments”, “events”, “fos_user”, “home”, “sessions”, “submission”), each with data in them already 
- f) Return to your terminal window for the next step 

 

### 8. Clone Spoutlet Github Repository on Instance

- a) Navigate to Apache’s default web page directory by typing “cd /var/www” and pressing enter 
- b) Return to the spoutlet2 github repository from step 7b (you will need a github account for the next step, create one if you don’t already have one) 
- c) On the repository, click the green “Clone of download” button and copy the given url 
- d) Return to your terminal window, you should still be navigated into the folder “/var/www” 
    - Type “sudo git clone *copied-url*” and hit enter (*url from step 8c) 
        - sudo git clone https://github.com/dnielsen/spoutlet2.git 

    - Enter your github user information to finish cloning the repository 

- e) After cloning the repository, type “ls” and hit enter, there should now be a “spoutlet2” directory present within “/var/www” 
- f) Navigate into the new project directory by typing “cd spoutlet2” and hitting enter 
- e) We will not be needing the “spoutlet2.sql” file, so delete it by typing “sudo rm spoutlet2.sql” and hitting enter 

### 8.5 Update Folder Permissions

- a) Type “sudo setfacl -R -m u:www-data:rX spoutlet2” and hit enter 
- b) Type “sudo setfacl -R -m u:www-data:rwX spoutlet2/app/cache spoutlet2/app/logs” and hit enter 
- c) Type “ sudo setfacl -dR -m u:www-data:rwX spoutlet2/app/cache spoutlet2/app/logs” and hit enter 
- d) Type “export SYMFONY_ENV=prod” and hit enter 

 

### 9. Update Spoutlet Project Requirements

- a) Open and edit the file “app/AppKernel.php” in the spoutlet2 directory 
    - cd spoutlet2 
    - sudo vi app/AppKernel.php 
    - and comment out the line “$bundles[] = new Sensio\Bundle\GeneratorBundle\SensioGeneratorBundle();” by adding two forward slashes in front of it  (“//”) 

- b) Update composer 
    - Type “sudo composer install --no-dev --optimize-autoloader” and hit enter, this can take up to a few minutes 
    - For all parameters, leave them blank and hit enter to accept the default, except for these: 
        - For “database_name” enter “Spoutlet” 
        - For “database_password” enter the password you created in step 6b 
        - For “mailer_transport” enter “gmail” 
        - For “mailer_host” enter “smtp.gmail.com” 
        - For “mailer_user” enter a valid gmail email address 
        - For “mailer_password” enter the email address’ respective password 
        - For “secret” enter “afb28c899101d12f6c5074bcef2fc2d0b0fa7084kk” 

- c) Ensure MySQL, phpMyAdmin, and Symfony are all communicating correctly by typing “sudo php app/console doctrine:schema:validate” and hitting enter, you should see an output verifying that the mapping and database schema are all correct 
- d) Clear cache by typing “sudo php app/console cache:clear --env=prod --no-debug” and hitting enter  
- e) Generate assets with “sudo php app/console assets:install --env=prod --no-debug” 

 

### 10. Change Default Apache Path to Spoutlet Web Page

- a) Navigate to apache default path configuration by typing “cd /etc/apache2/sites-available” 
- b) Type “sudo mv 000-default.conf default.bkp.conf” and hit enter 
- c) Open up a new file by typing “sudo vim 000-default.conf”, then paste in this code: 
  

```
<VirtualHost *:80>

    DocumentRoot /var/www/spoutlet2/web
    <Directory /var/www/spoutlet2/web>
        AllowOverride None
        Order Allow,Deny
        Allow from All

        <IfModule mod_rewrite.c>
            Options -MultiViews
            RewriteEngine On
            RewriteCond %{REQUEST_FILENAME} !-f
            RewriteRule ^(.*)$ app.php [QSA,L]
        </IfModule>
    </Directory>

    # uncomment the following lines if you install assets as symlinks
    # or run into problems when compiling LESS/Sass/CoffeScript assets
    # <Directory /var/www/project>
    #     Options FollowSymlinks
    # </Directory>

    ErrorLog /var/log/apache2/symfony_error.log
    CustomLog /var/log/apache2/symfony_access.log combined
</VirtualHost>

```

  

- d) Save and exit 
- e) Enable module rewrite by typing “sudo a2enmod rewrite” and hitting enter 
- f) Restart apache with “sudo systemctl restart apache2” 
- g) Open new web page and paste in the public DNS from step 5e, you should be directed to the Spoutlet2 Home Page 
    - For example [ec2-13-56-200-209.us-west-1.compute.amazonaws.com](http://ec2-13-56-200-209.us-west-1.compute.amazonaws.com)
