# Symfony essentials

## installing symfony new project & php bundle with composer 

Create new **web site like** project :

        composer create-project symfony/website-skeleton my-project

Create new **web application and api** project

        composer create-project symfony/skeleton my-project 

Go to my project repository       

        cd my-project 

Install server php bundle (optional)  

        composer require symfony/web-server-bundle --dev
        
launch local server at localhost:8000

        php bin/console server:run


## Storing your project on GitHub, Bitbucket ... / and clone an existing project
 
### Storing a local project on github

Go to my project repository ...       

        cd my-project 

and type this in a terminal :

        git init                                                            
        git add .
        git commit -m "Initial commit"

### Cloning and setting up an existing Symfony project
 

#### Clone the project from git 
 

        cd my-project/
        git clone ...

#### Install dependancies into vendor  

        cd my-project/
        composer install

## customize .env

configure database connexion in .env

## Checking for security vulnerabilities

    cd my-project/
    composer require sensiolabs/security-checker --dev

## Prepare symfony for creating routes


Needed if you want to have sorted routes from **annotations** :

        composer require annotations 

Create **routes** and add controller files via bin/console :
        
        php bin/console make:controller                                                 

## Bin console - usage

list all the **commands from bin/console** :

        php bin/console            

debug router:route via bin/console

        php bin/console debug:router         
        php bin/console debug:autowiring


## Install view for generating views templates

Install twig for generate views via php :

        composer require twig

## Install Profiler the Symfony debugger interface

Install profiler, the symfony debugger :

        composer require profiler  

Install the api rest support for symfony
        
        composer require api

easy remove a component or bundle ex : api 

        composer remove api


## Install Doctrine for create and manage sql databases via php

install doctrine :

        composer require doctrine

complete install with with the all doctrine tools  

        composer require symfony/orm-pack

make doctrine bundle ready to use (copy code)

        composer require symfony/maker-bundle --dev

list all the **command of doctrine** ORM sql database manager :

        php bin/console list doctrine                                                   
        php bin/console doctrine:database:create

Preparing the sql table shema :

        php bin/console make:entity

Creating the Database Tables/Schema :

        php bin/console make:migration
 
migrate database :

    console doctrine:migrations:migrate
    php bin/console make:entity --regenerate
