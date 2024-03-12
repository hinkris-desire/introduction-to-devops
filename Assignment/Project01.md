# SERVERS: Setting Up a LAMP STACK

This project aim is to create a server using the LAMP (Linux, Apache, MySQL, PHP/Python/Perl) STACK

## Steps
- Create a AWS account, we'll make use of the EC2 instance virtual machine to install every resources we need for the LAMP STACK. Setup the VM to run Linux Ubuntu DIstribution and run it via SSH or the EC2 instance connect. 
- Install the web server Apache using the command 
    ```
    sudo apt update 
    ```
    To update the dependecies in a fresh linux installation.
    ```
    sudo apt install apache2
    ```
    We then run this line to install apache2.
    ```
    sudo ufw allow in "Apache"
    ```
    ```
    sudo ufw enable
    ```
    ```
    sudo ufw status
    ```

    ![alt text](<../images/apache running.png>)

    Once its active, we check to see the landing page of the domain i.e. our ip address.
   
    ![alt text](<../images/landing page for the web swrver.png>)

- Install database manager MySQL and secure it. We'll use the script:
    ```
    sudo apt install mysql-server
    ```
    ```
    sudo msql
    ```

    ![alt text](<../images/mysql installed.png>)

    ```
    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password'; [to set a default password]

    ```
    Run this line to create a temporary password in order to securely authenticate the mysql_secure_installation
    ```
    sudo mysql_secure_installation
    ```
    ![alt text](<../images/mysql secure installation.png>)
    
    The password set in the previous line will be promted, then we select the level of password validation, level 1 will do. This will prompt you to change the inital password you set, as this setup will disable remote root login. After the setup, to avoid getting authentication error the line to run MySQL will be:
    ```
    sudo mysql -p
    ```
- Install PHP, our setup of LAMP STACK is almost complete, we have Apache to host the web service and content, then MySQL to store and manage our data. run the command to install php:
    ```
    sudo apt install php libapache2-mod-php php-mysql
    ```
    Once its finished, run the command:
    ```
    sudo php -v
    ```
    
    ![alt text](<../images/php installed.png>)

- Create a virtual host, after installing apache, a default landing page is setup in the directory var/www/html, if we're gong to create multiple virtual host, we then don't need to edit or replace the content in the default directory as we'll use it as backup and default directory whenever a client request doesn't match any other site. 
Create the directory:
    ```
    sudo mkdir /var/www/your_domain
    ```
    Assign ownership of the directory:
    ```
    sudo chown -R $USER:$USER /var/www/your_domain
    ```
    Open a new config file in the Apache sites-available directory and paste a bare-bone configuration script in the file we created, we can use any editor to do this and we used VIM editor:
    ```
    sudo vi /etc/apache2/sites-available/your_domain.conf
    ```
    
    ![alt text](<../images/create a virtual domain.png>)
    ```
    <VirtualHost *:80>
        ServerName your_domain
        ServerAlias www.your_domain 
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/your_domain
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>
    ```

    
    Now we enable the new virtual host:
    ```
    sudo a2ensite your_domain
    ```
    And then use this command to disable the default landing page:
    ```
    sudo a2dissite 000-default
    ```
    And then test the config file to check if it doesn't have any errors:
    ```
    sudo apache2ctl configtest
    ```
    ![alt text](<../images/virtual server setup.png>)
    Having finished with all the setup and config, our web root var/www/your_domain is still empty so we create a mock index.html file
    ```
    vi /var/www/your_domain/index.html
    ```
    ```
    <html>
        <head>
            <title>your_domain website</title>
        </head>
    <body>
        <h1>Hello World!</h1>

        <p>This is the landing page of <strong>your_domain</strong>.</p>
    </body>
    </html>   
    ```
    Once that is done then we can reload the domain name or IP address to see the new landing page.

    ![alt text](<../images/new landing page .png>)

With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file, you’ll need to edit the /etc/apache2/mods-enabled/dir.conf file and modify the order in which the index.php file is listed within the DirectoryIndex directive using the command:
```
sudo nano /etc/apache2/mods-enabled/dir.conf
```

Then add the index.php file in the directoryindex

![alt text](<../images/add php to directory index.png>)

After saving then reload the Apache so the changes takes effect.
- Testing the PHP processing, we create a PHP test script to confirm that Apache is able to handle and process requests for PHP files.  
    Create an index.php file in the web root folder:
    ```
    vi /var/www/your_domain/info.php
    ```
    ```
    <?php
    phpinfo();
    ```

    This line will print a page that provides information about your server from the perspective of PHP.

    ![alt text](<../images/PHP info.jpeg>)


# SERVERS: Setting up a LEMP STACK

LEMP [Linux, Nginx, MySQL, PHP], Much like the LAMP, follows the same process with the change being that we'll be using Nginx as the web server.

Just like we did with LAMP, 
Create a EC2 instance with Ubuntu as the OS.
Then update the package manager 
```
sudo apt update
```


- Installing Nginx Web Server, 
    ```
    sudo apt install nginx
    ```
    then enable uncomplicated firewall
    ```
    sudo ufw enable
    ```
    and run the command below to allow connections to Nginx
    ```
    sudo ufw allow 'Nginx HTTP'
    ```
    then check the status using,
    ```
    sudo ufw status
    ```
    ![alt text](<../images/ufw status nginx.png>)
    
    We'll then check if the server is running by accessing the VM IP address on our web browser.

    ![alt text](<../images/default landing page for nginx.png>)

    



    