# Project Structure¶

## root__

### config/

Contains... configuration!. You will configure routes, services and packages.
### src/

All your PHP code lives here.

### templates/

All your Twig templates live here.


        Most of the time, you'll be working in :

        * src/
        * templates/ 
        * config/

As you keep reading, you'll learn what can be done inside each of these.
So what about the other directories in the project?

### bin/

The famous bin/console file lives here (and other, less important executable files).

### var/

This is where automatically-created files are stored, like cache files (var/cache/) and logs (var/log/).

### vendor/

Third-party (i.e. "vendor") libraries live here! These are downloaded via the Composer package manager.

### public/

This is the document root for your project: you put any publicly accessible files here.