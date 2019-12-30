# Creating and Using Templates 

* Twig Templating Language
    * Twig Configuration
* Creating Templates
    * Template Naming
    * Template Location
    * Template Variables
    * Linking to Pages
    * Linking to CSS, JavaScript and Image Assets
    * The App Global Variable
* Rendering Templates
    * Rendering a Template in Controllers
    * Rendering a Template in Services
    * Rendering a Template in Emails
    * Rendering a Template Directly from a Route
    * Checking if a Template Exists
* Debugging Templates
    * Linting Twig Templates
    * Inspecting Twig Information
    * The Dump Twig Utilities
* Reusing Template Contents
    * Including Templates
    * Embedding Controllers
    * Template Inheritance and Layouts
* Output Escaping
* Template Namespaces
    * Bundle Templates
* Learn more

Render HTML from inside your application with Twig

## Twig Templating Language

    <!DOCTYPE html>
    <html>
        <head>
            <title>Welcome to Symfony!</title>
        </head>
        <body>
            <h1>{{ page_title }}</h1>

            {% if user.isLoggedIn %}
                Hello {{ user.name }}!
            {% endif %}

            {# ... #}
        </body>
    </html>

### Twig syntax 

* {{ ... }}, display the content of a variable or the result of an expression;
* {% ... %}, run some logic, such as a conditional or loop;
* {# ... #}, add comments to template (!HTML comments, not included in the rendered page).
* filters -> {{ title|upper }}

### Twig Configuration

* [Predefined tags](https://twig.symfony.com/doc/2.x/tags/index.html)
* [Filters](https://twig.symfony.com/doc/2.x/filters/index.html)
* [How to Write a custom Twig Extension](https://symfony.com/doc/current/templating/twig_extension.html)
* [Twig default extensions](https://symfony.com/doc/current/reference/twig_reference.html)
* [Twig configuration reference](https://symfony.com/doc/current/reference/configuration/twig.html) 

## Creating Templates

    {# templates/user/notifications.html.twig #}
    <h1>Hello {{ user_first_name }}!</h1>
    <p>You have {{ notifications|length }} new notifications.</p>

    Then, create a controller that renders this template and passes to it the needed variables :

    // src/Controller/UserController.php
    namespace App\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;

    class UserController extends AbstractController
    {
        // ...

        public function notifications()
        {
            // get the user information and notifications somehow
            $userFirstName = '...';
            $userNotifications = ['...', '...'];

            // the template path is the relative file path from `templates/`
            return $this->render('user/notifications.html.twig', [
                // this array defines the variables passed to the template,
                // where the key is the variable name and the value is the variable value
                // (Twig recommends using snake_case variable names: 'foo_bar' instead of 'fooBar')
                'user_first_name' => $userFirstName,
                'notifications' => $userNotifications,
            ]);
        }
    }

### Template Naming

* Use snake case (filenames and directories | blog_posts.twig ),
* the first extension (html, xml, etc.) is the final format that the template will generate.

### Template Location


### Template Variables


### Linking to Pages

templates/

#### example

    product/index.html.twig 

referring to the 

    <your-project>/templates/product/index.html.twig file.

### Template Variables

    <p>{{ user.name }} added this comment on {{ comment.publishedAt|date }}</p>

When using the foo.bar notation, Twig tries to get the value of the variable in the following order:

1. $foo['bar'] (array and element);
2. $foo->bar (object and public property);
3. $foo->bar() (object and public method);
4. $foo->getBar() (object and getter method);
5. $foo->isBar() (object and isser method);
6. $foo->hasBar() (object and hasser method);
7. If none of the above exists, use null.


### Linking to Pages

Consider the following routing configuration :

    // src/Controller/BlogController.php
    namespace App\Controller;

    // ...
    use Symfony\Component\Routing\Annotation\Route;

    class BlogController extends AbstractController
    {
        /**
        * @Route("/", name="blog_index")
        */
        public function index()
        {
            // ...
        }

        /**
        * @Route("/article/{slug}", name="blog_post")
        */
        public function show(string $slug)
        {
            // ...
        }
    }

    <a href="{{ path('blog_index') }}">Homepage</a>

    {# ... #}

    {% for post in blog_posts %}
        <h1>
            <a href="{{ path('blog_post', {slug: post.slug}) }}">{{ post.title }}</a>
        </h1>

        <p>{{ post.excerpt }}</p>
    {% endfor %}


### Linking to CSS, JavaScript and Image Assets

If a template needs to link to a static asset, use :

    composer require symfony/asset


    {# the image lives at "public/images/logo.png" #}
    <img src="{{ asset('images/logo.png') }}" alt="Symfony!"/>

    {# the CSS file lives at "public/css/blog.css" #}
    <link href="{{ asset('css/blog.css') }}" rel="stylesheet"/>

    {# the JS file lives at "public/bundles/acme/js/loader.js" #}
    <script src="{{ asset('bundles/acme/js/loader.js') }}"></script>

& make your application more portable.

#### If you need absolute URLs : 

    <img src="{{ absolute_url(asset('images/logo.png')) }}" alt="Symfony!"/>
    <link rel="shortcut icon" href="{{ absolute_url('favicon.png') }}">

### The App Global Variable

    <p>Username: {{ app.user.username ?? 'Anonymous user' }}</p>
    {% if app.debug %}
        <p>Request method: {{ app.request.method }}</p>
        <p>Application Environment: {{ app.environment }}</p>
    {% endif %}

The app variable (instance of AppVariable) gives access to these variables:

1. **app.user** : Current user object or null if the user is not authenticated.
2. **app.request** : Request object that stores the current request data (this can be a sub-request or a regular request).
3. **app.session** : Session object that represents the current user's session or null if there is none.
4. **app.flashes** : Array of all flash messages stored in session or messages of some type (e.g. app.flashes('notice')).
5. **app.environment** : Name of the current configuration environment (dev, prod, etc).
6. **app.debug** : True if in debug mode. False otherwise.
7. **app.token** : TokenInterface object representing the security token.

In addition to the global app variable injected by Symfony, 
you can also inject variables automatically to all Twig templates.


## Rendering Templates

### Rendering a Template in Controllers

    // src/Controller/ProductController.php
    namespace App\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;

    class ProductController extends AbstractController
    {
        public function index()
        {
            // ...

            return $this->render('product/index.html.twig', [
                'category' => '...',
                'promotions' => ['...', '...'],
            ]);
        }
    }

### Rendering a Template in Services

see : service autowiring  

    // src/Service/SomeService.php
    namespace App\Service;

    use Twig\Environment;

    class SomeService
    {
        private $twig;

        public function __construct(Environment $twig)
        {
            $this->twig = $twig;
        }

        public function someMethod()
        {
            // ...

            $htmlContents = $this->twig->render('product/index.html.twig', [
                'category' => '...',
                'promotions' => ['...', '...'],
            ]);
        }
    }

### Rendering a Template in Emails

Read the docs about the [mailer and Twig integration](https://symfony.com/doc/current/mailer.html#mailer-twig).

### Rendering a Template Directly from a Route

    // config/routes.php
    use Symfony\Bundle\FrameworkBundle\Controller\TemplateController;
    use Symfony\Component\Routing\Loader\Configurator\RoutingConfigurator;

    return function (RoutingConfigurator $routes) {
        $routes->add('acme_privacy', '/privacy')
            ->controller(TemplateController::class)
            ->defaults([
                // the path of the template to render
                'template'  => 'static/privacy.html.twig',

                // special options defined by Symfony to set the page cache
                'maxAge'    => 86400,
                'sharedAge' => 86400,

                // optionally you can define some arguments passed to the template
                'site_name' => 'ACME',
                'theme' => 'dark',
            ])
        ;
};

### Checking if a Template Exists

    // in a controller extending from AbstractController
    $loader = $this->get('twig')->getLoader();

    // in a service using autowiring
    use Twig\Environment;

    public function __construct(Environment $twig)
    {
        $loader = $twig->getLoader();
    }

then :

    if ($loader->exists('theme/layout_responsive.html.twig')) {
        // the template exists, do something
        // ...
    }

## Debugging Templates

### Linting Twig Templates

    php bin/console lint:twig
    php bin/console lint:twig templates/email/
    php bin/console lint:twig templates/article/recent_list.html.twig
    php bin/console lint:twig --show-deprecations templates/email/

### Inspecting Twig Information

    php bin/console debug:twig
    php bin/console debug:twig --filter=date
    php bin/console debug:twig @Twig/Exception/error.html.twig

### The Dump Twig Utilities

    composer require symfony/var-dumper



    {# templates/article/recent_list.html.twig #}
    {# the contents of this variable are sent to the Web Debug Toolbar
    instead of dumping them inside the page contents #}
    {% dump articles %}

    {% for article in articles %}
        {# the contents of this variable are dumped inside the page contents
        and they are visible on the web page #}
        {{ dump(article) }}

        <a href="/article/{{ article.slug }}">
            {{ article.title }}
        </a>
    {% endfor %}

Dump var only avaible on dev mode, see 
[configure environment](https://symfony.com/doc/current/configuration.html#configuration-environments)
In prod environment, you will see a PHP error.

## Reusing Template Contents

### Including Templates

    {# templates/blog/index.html.twig #}
    {# ... #}
    <div class="user-profile">
        <img src="{{ user.profileImageUrl }}"/>
        <p>{{ user.fullName }} - {{ user.email }}</p>
    </div>


    {# templates/blog/index.html.twig #}
    {# ... #}
    {{ include('blog/_user_profile.html.twig') }}


    {# templates/blog/index.html.twig #}

    {# ... #}
    {{ include('blog/_user_profile.html.twig', {user: blog_post.author}) }}

### Embedding Controllers

better alternative is to embed the result of executing some controller with the render() and controller() Twig functions.


    // src/Controller/BlogController.php
    namespace App\Controller;
    // ...
    class BlogController extends AbstractController
    {
        public function recentArticles($max = 3)
        {
            // get the recent articles somehow (e.g. making a database query)
            $articles = ['...', '...', '...'];

            return $this->render('blog/_recent_articles.html.twig', [
                'articles' => $articles
            ]);
        }
    }


    {# templates/blog/_recent_articles.html.twig #}
    {% for article in articles %}
        <a href="{{ path('blog_show', {slug: article.slug}) }}">
            {{ article.title }}
        </a>
    {% endfor %}



    {# templates/base.html.twig #}

    {# ... #}
    <div id="sidebar">
        {# if the controller is associated with a route, use the path() or url() functions #}
        {{ render(path('latest_articles', {max: 3})) }}
        {{ render(url('latest_articles', {max: 3})) }}

        {# if you don't want to expose the controller with a public URL,
        use the controller() function to define the controller to execute #}
        {{ render(controller(
            'App\\Controller\\BlogController::recentArticles', {max: 3}
        )) }}
    </div>


    // config/packages/framework.php
    $container->loadFromExtension('framework', [
        // ...
        'fragments' => ['path' => '/_fragment'],
    ]);


### Template Inheritance and Layouts

The concept of Twig template inheritance is similar to PHP class inheritance. 
You define a parent template that other templates can extend 
from and child templates can override parts of the parent template.

    {# templates/base.html.twig #}
    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="UTF-8">
            <title>{% block title %}My Application{% endblock %}</title>
        </head>
        <body>
            <div id="sidebar">
                {% block sidebar %}
                    <ul>
                        <li><a href="{{ path('homepage') }}">Home</a></li>
                        <li><a href="{{ path('blog_index') }}">Blog</a></li>
                    </ul>
                {% endblock %}
            </div>

            <div id="content">
                {% block body %}{% endblock %}
            </div>
        </body>
    </html>

The [Twig block tag](https://twig.symfony.com/doc/2.x/tags/block.html) can be overridden in the child templates.

    {# templates/blog/layout.html.twig #}
    {% extends 'base.html.twig' %}

    {% block body %}
        <h1>Blog</h1>

        {% block content %}{% endblock %}
    {% endblock %}



    {# templates/blog/index.html.twig #}
    {% extends 'blog/layout.html.twig' %}

    {% block title %}Blog Index{% endblock %}

    {% block content %}
        {% for article in articles %}
            <h2>{{ article.title }}</h2>
            <p>{{ article.body }}</p>
        {% endfor %}
    {% endblock %}

## Output Escaping (example js)

<p>Hello {{ name }}</p>
    {# if 'name' is '<script>alert('hello!')</script>', Twig will output this:
    '<p>Hello &lt;script&gt;alert(&#39;hello!&#39;)&lt;/script&gt;</p>' #}

    <h1>{{ product.title|raw }}</h1>
    {# if 'product.title' is 'Lorem <strong>Ipsum</strong>', Twig will output
    exactly that instead of 'Lorem &lt;strong&gt;Ipsum&lt;/strong&gt;' #}
    Filter raw prevent escaping var if needed
    see : [Twig output escaping docs](https://twig.symfony.com/doc/2.x/api.html#escaper-extension)

## Template Namespaces
you may need to store some or all of them in different directories. than /templates
for that use twig.paths :

    // config/packages/twig.php
    $container->loadFromExtension('twig', [
        // ...
        'paths' => [
            // directories are relative to the project root dir (but you
            // can also use absolute directories)
            'email/default/templates' => null,
            'backend/templates' => null,
        ],
    ]);

first symfony get templates in the twig.paths directories
and then falls back to the default template directory (templates/).

### Config namespaces in config/packages/twig.php

// config/packages/twig.php
$container->loadFromExtension('twig', [
    // ...
    'paths' => [
        'email/default/templates' => 'email',
        'backend/templates' => 'admin',
    ],
]);

A single Twig namespace can be associated with more than one template directory. 
In that case, the order in which paths are added is important because 
Twig will start looking for templates from the first defined path.

### Bundle Templates

If you install packages/bundles in your application, 
they may include their own Twig templates (in the Resources/views/ directory of each bundle). 
To avoid messing with your own templates, 
Symfony adds bundle templates under an automatic namespace created after the bundle name.

For example, the templates of a bundle called AcmeFooBundle are available under the AcmeFoo namespace. 
If this bundle includes the template 
<your-project>/vendor/acmefoo-bundle/Resources/views/user/profile.html.twig, 
you can refer to it as @AcmeFoo/user/profile.html.twig.

## Learn more

1. [How to Use PHP instead of Twig for Templates](https://symfony.com/doc/current/templating/PHP.html)
2. [How to Inject Variables Automatically into all Templates](https://symfony.com/doc/current/templating/global_variables.html)
3. [How to Embed Asynchronous Content with hinclude.js](https://symfony.com/doc/current/templating/hinclude.html)
4. [How to Write a custom Twig Extension](https://symfony.com/doc/current/templating/twig_extension.html)