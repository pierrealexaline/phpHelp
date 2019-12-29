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

* 1 $foo['bar'] (array and element);
* 1 $foo->bar (object and public property);
* 1 $foo->bar() (object and public method);
* 1 $foo->getBar() (object and getter method);
* 1 $foo->isBar() (object and isser method);
* 1 $foo->hasBar() (object and hasser method);
* 1 If none of the above exists, use null.
