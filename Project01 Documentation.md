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
    ![alt text](images/apache%20running.png)
    Once its active, we check to see the landing page of the domain i.e. our ip address.
    ![alt text](images/landing%20page%20for%20the%20web%20swrver.png)

- Install database manager MySQL and secure it. We'll use the script:
    ```
    sudo apt install mysql-server
    ```
    ```
    sudo msql
    ```
    ![alt text](images/mysql%20installed.png)
    ```
    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password'; [to set a default password]

    ```
    Run this line to create a temporary password in order to securely authenticate the mysql_secure_installation
    ```
    sudo mysql_secure_installation
    ```
    ![alt text](images/mysql%20secure%20installation.png)
    
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
    ![alt text](images/php%20installed.png)
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
    ![alt text](images/create%20a%20virtual%20domain.png)
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
    ![alt text](images/create%20a%20virtual%20domain.png)
    
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
    ![alt text](images/virtual%20server%20setup.png)
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

    ![alt text](images/new%20landing%20page%20.png)



