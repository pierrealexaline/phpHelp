# The Symfony MakerBundle


    composer require symfony/maker-bundle --dev
    php bin/console list make


Available commands for the "make" namespace :

* **make:auth**                   Creates a Guard authenticator of different flavors
* **make:command**                Creates a new console command class
* **make:controller**             Creates a new controller class
* **make:crud**                   Creates CRUD for Doctrine entity class
* **make:entity**                 Creates or updates a Doctrine entity class, and optionally an API Platform resource
* **make:fixtures**               Creates a new class to load Doctrine fixtures
* **make:form**                   Creates a new form class
* **make:functional-test**        Creates a new functional test class
* **make:migration**              Creates a new migration based on database changes
* **make:registration-form**      Creates a new registration form system
* **make:serializer:encoder**     Creates a new serializer encoder class
* **make:serializer:normalizer**  Creates a new serializer normalizer class
* **make:subscriber**             Creates a new event subscriber class
* **make:twig-extension**         Creates a new Twig extension class
* **make:unit-test**              Creates a new unit test class
* **make:user**                   Creates a new security user class
* **make:validator**              Creates a new validator and constraint class
* **make:voter**                  Creates a new security voter class

    php bin/console make:controller --help

## Example : Make controller via php console/bin

### Description:
  Creates a new controller class

### Usage :
  make:controller [options] [--] [<controller-class>]

### Arguments :
  controller-class      Choose a name for your controller class (e.g. DeliciousPizzaController)

### Options :
      --no-template     Use this option to disable template generation
  -h, --help            Display this help message
  -q, --quiet           Do not output any message
  -V, --version         Display this application version
      --ansi            Force ANSI output
      --no-ansi         Disable ANSI output
  -n, --no-interaction  Do not ask any interactive question
  -e, --env=ENV         The Environment name. [default: "dev"]
      --no-debug        Switches off debug mode.
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

### Help :
* The make:controller command generates a new controller class.

    php bin/console make:controller CoolStuffController

* If the argument is missing, the command will ask for the controller class name interactively.
  
* You can also generate the controller alone, without template with this option :

  php bin/console make:controller --no-template


## Configuration

  config/packages/dev/maker.yaml

create this file if you need to configure anything

you can only configure the root namespace that is used to "guess" what classes you want to generate :

    maker:
        # tell MakerBundle that all of your classes lives in an
        # Acme namespace, instead of the default App
        # (e.g. Acme\Entity\Article, Acme\Command\MyCommand, etc)
        root_namespace: 'Acme'


## Creating your Own Makers :

To do that, you should create a class that extends [**AbstractMaker**](https://github.com/symfony/maker-bundle/blob/master/src/Maker/AbstractMaker.php) in your src/Maker/ directory.

## Overriding the Generated Code

Existing commands can be improved and new commands can be added, for that reason, in general, the generated code cannot be modified.