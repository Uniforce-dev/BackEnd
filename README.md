# BackEnd Symfony
BACK END API & BDD for UNI-FORCE Project

## QUICK START

Requirements : 
 - Started mySql server (Wamp, Xamp, Docker, etc...) 
 - Composer
 - Symfony CLI : https://symfony.com/download 
 
CLone Back end project in any folder of your choice

Run ```symfony serve -d --no-tls ```
(```-d ``` option allows you to close CLI window while server is runnning)
(```--no-tls ``` deactivates tls encryption)

API Server is running on localhost:8000/api 

Check mysql port in .env file to set BDD connection address

______


## Complete environment installation with DOCKER, WSL2, usefull commands

________________

# DOCKER

[https://medium.com/@fred.gauthier.dev/web-development-environment-with-wsl-2-and-docker-for-symfony-5860704e127a]

## Docker installation

### In Windows
go to https://hub.docker.com/editions/community/docker-ce-desktop-windows/ and install Docker Desktop for Windows.

Go to the Docker’s settings and check if “Use the WSL 2 based engine…” option is checked

Now, we have to install Docker inside the linux distribution. Go to https://docs.docker.com/engine/install/debian/ for a Debian installation

### In a linux terminal :

sudo apt-get update -y && sudo apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common

Now, add Docker’s official GPG key :

> curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -

Add the repository (x86_64/amd64 based)

> sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"

Install Docker Engine :

> sudo apt-get update -y && sudo apt-get install -y docker-ce docker-ce-cli containerd.io

And add your user to the Docker’s group :

> sudo usermod -a -G docker $USER

And check if docker is running :

> docker -v



## docker-compose 

To work with Docker, you create container with a command line, but a command line is not easy to handle when you want to restart often your environment like you can do in a web environment.

Docker-compose is like a recipe for Docker and it’s more friendly to use than command line. For Web development, I like to have a Docker-compose for each project I work. It’s easy to start, restart and update and each environment is independant.

You can download and install Docker-compose like this :

> sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

And make sure Docker-compose is executable :

> sudo chmod +x /usr/local/bin/docker-compose

You can check if Docker-compose is working :

> docker-compose -v

Everything is working ! Now you can work with Docker on WSl 2 !


## Docker-compose

Create a directory
add file docker-compose.yml

    # Use root/example as user/password credentials 
    version: '3.1'

    services:

    db:
        image: mysql
        command: --default-authentication-plugin=mysql_native_password
        ports:
        - 3306:3306
        environment:
        MYSQL_ROOT_PASSWORD: root
      
    phpmyadmin:
        image: phpmyadmin
        container_name: phpmyadmin
        environment:
        - PMA_ARBITRARY=1
        ports:
        - 8080:80
        volumes:
        - /sessions

Check all parameters: password, port etc

COMMANDS in your directory
These commands creates & starts the 2 containers described in docker-compose.yml

> docker-compose up -d   (-d pour les t)
> docker-compose start
> docker ps


see PHPMYADMIN

> http://localhost:8080/


## PROJET SYMFONY BACK END


### install composer for symfony

First, I need to install PHP and Composer to initiate the Symfony project

> sudo apt install php php-xml php-zip unzip

php-xml and php-zip are requiered to install Symfony, unzip is recommended by Composer.

[https://getcomposer.org/download/]

> php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"

This installer script will simply check some php.ini settings, warn you if they are set incorrectly, and then download the latest composer.phar in the current directory. The 4 lines above will, in order:

    Download the installer to the current directory
    Verify the installer SHA-384, which you can also cross-check here
    Run the installer
    Remove the installer


### clone back end project

> composer install


### config


fichier .env
DATABASE_URL="mysql://root:root@127.0.0.1:3306/uniforce"

server_version pour le serveur mysql soit dans le DATABASE_URL ci-dessus
DATABASE_URL="mysql://root:root@127.0.0.1:3306/uniforce?serverVersion='8.0.23"

soit ici :

    fichier doctrine.yml

    doctrine:
        dbal:
            url: '%env(resolve:DATABASE_URL)%'

            # IMPORTANT: You MUST configure your server version,
            # either here or in the DATABASE_URL env var (see .env file)
            server_version: '8.0.23'
        orm:
            auto_generate_proxy_classes: true
            naming_strategy: doctrine.orm.naming_strategy.underscore_number_aware
            auto_mapping: true
            mappings:
                App:
                    is_bundle: false
                    type: annotation
                    dir: '%kernel.project_dir%/src/Entity'
                    prefix: 'App\Entity'
                    alias: App


### Start symfony project

> symfony serve --no-tls
> symfony serve --no-tls -d si on le veut en tâche de fond

[http://localhost:8000/] ACCUEIL SYMFONY

[http://localhost:8000/api] API BACK END
[http://127.0.0.1:8080/] (do not work on WSL2)

generate BDD automatically

> BackEnd git:(master) ✗ php bin/console doctrine:database:create
> BackEnd git:(master) ✗ php bin/console make:migration
> BackEnd git:(master) ✗ php bin/console doctrine:database:migrate


## BUGS INSTALL

chemin php.ini
access mysql in docker

