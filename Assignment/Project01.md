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

    - Installing the database manager MySQL,
    ```
    sudo apt install mysql-server
    ```
    ![alt text](<../images/mysql installe on LEMP.png>)

    Now to complete the configuration,
    ```
    sudo mysql_secure_installation
    ```
    After installation, we can either choose to change the authentication method between auth_socket and mysql_native_password
    open MySql using the command 
    ```
    sudo mysql
    ```
    then run the command below to check which authentication method is currently being used.
    ```
    SELECT user,authentication_string,plugin,host FROM mysql.user;
    ```
    ![alt text](<../images/mysql auth in LEMP.png>)

    The current authentication method is auth_socket, so to change it to native password, we run the command 
    ,
    ```
    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
    ```
    Afterward we run the command to reload the table.
    ```
    FLUSH PRIVILEGES;
    ```
    then check the authentication method used,
    ```
    SELECT user,authentication_string,plugin,host FROM mysql.user;
    ```
    ![alt text](<../images/mysql auth method list.png>)

    Once that is successfully configured, we'll have to use the command below to run MySql
    ```
    sudo mysql -p
    ```
    ![alt text](<../images/mysql running in LEMP.png>)

    Nginx does not contain native PHP processing like some other web servers, we will need to install php-fpm [fastCGI process manager].

    We would need to add Ubuntu universal repository in order to install php-fpm using the command,
    ``` 
    sudo add-apt-repository universe
    ```
    ![alt text](<../images/additional repo .png>)

    We then install php-fpm together with a helper package php-mysql which allows php to connect with the database backend.
    ```
    sudo apt install php-fpm php-mysql
    ```
    ![alt text](<../images/php-fpm together with php-mysql.png>)

    Having installed all the LEMP components [Linux OS, Nginx, MySql, PHP], we still need to make some configurations in order to program Nginx to use the PHP processor for dynamic contents.
    Thsi is done on the server block/ virtual host on the web server, to do this we create a new server block in the directory /etc/nginx/sites-available/.
    ```
    sudo nano /etc/nginx/sites-available/your_domain
    ```
    Paste the configuration script in the file created, edit the domain name to yours and repalce the php-fpm version (in my case its version 8.3) 
    ```
    server {
        listen 80;
        root /var/www/your_domain;
        index index.php index.html index.htm index.nginx-debian.html;
        server_name your_domain;

        location / {
                try_files $uri $uri/ =404;
        }

        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
        }

        location ~ /\.ht {
                deny all;
        }
    }
    ```
    ![alt text](<../images/Nginx config file.png>)

    Here’s what each directives and location blocks does:

    **Listen** — Defines what port Nginx will listen on. In this case, it will listen on port 80, the default port for HTTP.

    **Root** — Defines the document root where the files served by the website are stored.

    **Index** — Configures Nginx to prioritize serving files named index.php when an index file is requested if they’re available.

    **Server_name** — Defines which server block should be used for a given request to your server. [Point this directive to your server’s domain name or public IP address.]

    **Location /** — The first location block includes a try_files directive, which checks for the existence of files matching a URI request. If Nginx cannot find the appropriate file, it will return a 404 error.

    **Location ~ \.php$** — This location block handles the actual PHP processing by pointing Nginx to the fastcgi-php.conf configuration file and the php7.2-fpm.sock file, which declares what socket is associated with php-fpm.

    **Location ~ /\.ht** — The last location block deals with .htaccess files, which Nginx does not process. By adding the deny all directive, if any .htaccess files happen to find their way into the document root they will not be served to visitors.
    
    We then enable the new server block by linking the new server block configuration file to the /etc/nginx/sites-enabled/ directory.
    ```
    sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
    ```
    Then, unlink the default configuration file from the /sites-enabled/ directory.
    ```
    sudo unlink /etc/nginx/sites-enabled/default
    ```

    If we ever need to restore the default configuration we just need to use the same command but with the default file.
    ```
    sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/
    ```
    Then we test the configuration for errors.
    ```
    sudo nginx -t
    ```
    ![alt text](<../images/test nginx new config file.png>)
    We then reload the Nginx to update the changes made.
    ```
    sudo systemctl reload nginx
    ```

- Creating a PHP File to Test Configuration to test and confirm that Nginx can handle .php files off the PHP processor, as we're done with setting up the LEMP components.
    ```
    sudo nano /var/www/html/info.php
    ```
    after creating the file, paste the code,
    ```
    <?php
    phpinfo();
    ```
    Now we can visit the page via the server domain name or the IP address.

    ```
    http://ip-address/info.php
    ```
    ![alt text](<../images/php config page.png>)



    













    