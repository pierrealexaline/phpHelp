# Symfony CLI version v4.9.0 (c) 2017-2019 Symfony SAS

Symfony CLI helps developers manage projects, from local code to remote infrastructure

## Usage:
  symfony [global options] <command> [command options] [arguments...]

## Global options:
* --help, -h                         Show help [default: false]
* --quiet, -q                        Do not output any message
* -v|vv|vvv, --verbose, --log-level  Increase the verbosity of messages: 1 for normal output, 2 and 3 for more verbose outputs and 4 for debug [default: 1]
* -V                                 Print the version [default: false]

## Available commands:

### account                                                                   
* **account:info**                                                                Show info about your Symfony account
* **account:ips**                                                                List SymfonyCloud IPs for use in scripts
* **account:login, login**                                                        Log in with your SymfonyConnect account
* **account:logout, logout**                                                      Logout from your SymfonyConnect account
* **account:ssh:key:add**                                                         Add an SSH key
* **account:ssh:key:remove**                                                      Remove an SSH key
* **account:ssh:keys**                                                            List project's SSH keys
### billing                                                                   
* **billing:update:card**                                                         Update card used for billing
### domain                                                                    
* **domain:attach**                                                               Attach a domain
* **domain:cname, cname**                                                         Display the CNAME value to use in DNS entries
* **domain:default**                                                              Display/Set the default domain
* **domain:detach**                                                               Detach a domain
* **domain:get**                                                                  Show detailed information for a domain
* **domain:list, domains**                                                     List domains
* **domain:update**                                                               Update a domain
### env                                                                       
* **env:activate**                                                                Activate one or several environments
* **env:activity, activity, history**                                             Display activity history for an environment
* **env:checkout**                                                                Checkout a SymfonyCloud environment as a local Git branch
* **env:cp**                                                                      Copy files/folders between your host and the local machine
* **env:create**                                                                  Create an environment
* **env:cron, cron**                                                              Run a cron for the environment
* **env:db:dump, db:dump**                                                        Dump remote database
* **env:db:size, db:size**                                                        Estimate the disk usage of a database
* **env:debug, debug**                                                           Debug an environment by switching Symfony to the debug mode temporarily
* **env:delete**                                                                  Delete an environment
* **env:deploy, deploy**                                                          Deploy an environment
* **env:fpm:status, php-fpm-status**                                              Get PHP-FPM status
* **env:link**                                                                    Link a local branch to an environment
* **env:list, envs**                                                              List environments
* **env:logs, log, logs**                                                         Display logs for an environment
* **env:mount:list, mount:list**                                                  Get a list of mounts
* **env:mount:size, mount:size**                                                  Check the disk usage of mounts
* **env:redeploy, redeploy**                                                      Redeploy an environment, shortcut for deploy --reuse-build 
* **env:rsync**                                                                   Rsync files/folders between your host and the local machine
* **env:setting:list, env:settings**                                              List settings for an environment
* **env:setting:set**                                                             Change setting value for an environment
* **env:snapshot:create**                                                         Make a snapshot of an environment
* **env:snapshot:list, env:snapshots**                                            List project snapshots
* **env:snapshot:restore**                                                        Restore an environment snapshot
* **env:sql, sql**                                                                Run SQL on the remote database
* **env:ssh, ssh**                                                                Open an SSH connection to the app container
* **env:sync**                                                                    Synchronize environment's data from the parent one
* **env:urls, urls**                                                              Show public URLs for this environment
* **env:validate**                                                                Validate an environment configuration
### integration                                                               
* **integration:add**                                                             Configure an integration with a third-party service
* **integration:delete**                                                          Delete an integration
* **integration:get**                                                             Display details for an integration
* **integration:list, integrations**                                              List project integrations
### local                                                                     
* **local:check:requirements, check:requirements, check:req**                     Check requirements for Symfony projects.
* **local:check:security, security:check, check:security, local:security:check**  Check security issues in project dependencies
* **local:new, new**                                                              Create a new Symfony project
* **local:php:list**                                                              List locally available PHP versions
* **local:php:refresh**                                                           Auto-discover the list of available PHP version
* **local:php:wrappers:install, php:wrappers:install**                            Install wrappers for PHP binaries
* **local:proxy:domain:attach, proxy:domain:attach**                             Attach a local domain for the proxy
* **local:proxy:domain:detach, proxy:domain:detach**                              Detach domains from the proxy
* **local:proxy:start, proxy:start**                                              Start the local proxy server (local domains support)
* **local:proxy:stop, proxy:stop**                                                Stop the local proxy server
* **local:run, run**                                                              Run a program with environment variables set depending on the current context
* **local:server:ca:install, server:ca:install**                                  Create a local Certificate Authority for serving HTTPS
* **local:server:ca:uninstall, server:ca:uninstall**                              Uninstall the local Certificate Authority
* **local:server:list, server:list**                                              List all running local web servers
* **local:server:log, server:log**                                                Display local web server logs
* **local:server:prod, server:prod**                                              Switch a project to use Symfony's production environment
* **local:server:start, serve, server:start**                                     Run a local web server
* **local:server:status, server:status**                                          Get the local web server status
* **local:server:stop, server:stop**                                              Stop the local web server
### open                                                                      
* **open:docs**                                                                   Open the online Web documentation
* **open:local**                                                                  Open the local project in a browser
* **open:local:webmail**                                                          Open the local project mail catcher web interface in a browser
* **open:remote**                                                                 Open the remote project in a browser
* **open:support, open:issue**                                                    Open the web support page
### project                                                                   
* **project:create**                                                              Create a new project
* **project:delete**                                                              Delete current project
* **project:deploy-key**                                                          Display the SSH deploy key of a project
* **project:edit, project:update**                                                Edit a project's quota
* **project:git-url**                                                             Display the Git remote URL of a project
* **project:info**                                                                Display information about the current project
* **project:init, init**                                                          Initialize a new project using templates
* **project:link, link**                                                          Link current git repository to a SymfonyCloud project
* **project:list, projects**                                                      List active projects
* **project:rename**                                                              Rename a project
* **project:scale**                                                               Scale a project up or down
* **project:unlink, unlink**                                                      Unlink current git repository
### self                                                                      
* **self:about**                                                                  Display legal information
* **self:cleanup**                                                                Cleanup previous versions from CLI updates
* **self:help, help, list**                                                       Display help for a command or a category of commands
* **self:rollback**                                                               Rollback the CLI to the previous version
* **self:update, self-update**                                                    Update the CLI to the latest version
* **self:version, version**                                                       Display the application version
### tunnel                                                                    
* **tunnel:close**                                                                Close SSH tunnels
* **tunnel:info**                                                                 View relationships for SSH tunnels
* **tunnel:list, tunnels**                                                        List SSH tunnels
* **tunnel:open**                                                                 Open SSH tunnels to the app's services
### user                                                                      
* **user:add**                                                                    Add a user to the project
* **user:list, users**                                                            List project users
* **user:remove**                                                                 Remove a user from the project
### var                                                                       
* **var:delete**                                                                  Delete one or more variables from a project or an environment
* **var:disable**                                                                 Disable one or more variables for an environment
* **var:enable**                                                                  Enable one or more variables for an environment
* **var:export**                                                                  Export environment variables depending on the current context
* **var:get, vars**                                                               List variables
* **var:set**                                                                     Set one or multiple variables for a project or an environment

## Available wrappers:
Runs PHP (version depends on project's configuration).
Environment variables to use SymfonyCloud relationships or Docker services are automatically defined.

* composer                                               Runs Composer without memory limit
* console                                                Runs the Symfony Console (bin/console) for current project
* php, pecl, pear, php-fpm, php-cgi, php-config, phpdbg  Runs the named binary using the configured PHP version
