# Controller  
* A Simple Controller
    * Mapping a URL to a Controller
* The Base Controller Class & Services
    * Generating URLs
    * Redirecting
    * Rendering Templates
    * Fetching Services
* Generating Controllers
* Managing Errors and 404 Pages
* The Request object as a Controller Argument
* Managing the Session
    * Flash Messages
* The Request and Response Object
    * Accessing Configuration Values
    * Returning JSON Response
    * Streaming File Responses
* Final Thoughts
* Keep Going!
* Learn more about Controllers
 

## A Simple Controller

    // src/Controller/LuckyController.php
    namespace App\Controller;

    use Symfony\Component\HttpFoundation\Response;
    use Symfony\Component\Routing\Annotation\Route;

    class LuckyController
    {
        /**
        * @Route("/lucky/number/{max}", name="app_lucky_number")
        */
        public function number($max)
        {
            $number = random_int(0, $max);

            return new Response(
                '<html><body>Lucky number: '.$number.'</body></html>'
            );
        }
    }

### Mapping a URL to a Controller

    http://localhost:8000/lucky/number/100

## The Base Controller Class & Services

    // src/Controller/LuckyController.php
    namespace App\Controller;

    + use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;

    - class LuckyController
    + class LuckyController extends AbstractController
    {
        // ...
    }

### Generating URLs

    $url = $this->generateUrl('app_lucky_number', ['max' => 10]);

### Redirecting

    use Symfony\Component\HttpFoundation\RedirectResponse;

    // ...
    public function index()
    {
        // redirects to the "homepage" route
        return $this->redirectToRoute('homepage');

        // redirectToRoute is a shortcut for:
        // return new RedirectResponse($this->generateUrl('homepage'));

        // does a permanent - 301 redirect
        return $this->redirectToRoute('homepage', [], 301);

        // redirect to a route with parameters
        return $this->redirectToRoute('app_lucky_number', ['max' => 10]);

        // redirects to a route and maintains the original query string parameters
        return $this->redirectToRoute('blog_show', $request->query->all());

        // redirects externally
        return $this->redirect('http://symfony.com/doc');
    }

### Rendering Templates

    // renders templates/lucky/number.html.twig
    return $this->render('lucky/number.html.twig', ['number' => $number]);

### Fetching Services

    use Psr\Log\LoggerInterface;
    // ...

    /**
    * @Route("/lucky/number/{max}")
    */
    public function number($max, LoggerInterface $logger)
    {
        $logger->info('We are logging!');
        // ...
    }


    php bin/console debug:autowiring


    // config/services.php
    use App\Controller\LuckyController;
    use Symfony\Component\DependencyInjection\Reference;

    $container->register(LuckyController::class)
        ->setPublic(true)
        ->setBindings([
            '$logger' => new Reference('monolog.logger.doctrine'),
            '$projectDir' => '%kernel.project_dir%'
        ])
    ;

## Generating Controllers

To save time, install [Symfony Maker](https://symfony.com/doc/current/bundles/SymfonyMakerBundle/index.html) and tell Symfony to generate a new controller class:

    php bin/console make:controller BrandNewController

        created: src/Controller/BrandNewController.php
        created: templates/brandnew/index.html.twig

    php bin/console make:crud Product

        created: src/Controller/ProductController.php
        created: src/Form/ProductType.php
        created: templates/product/_delete_form.html.twig
        created: templates/product/_form.html.twig
        created: templates/product/edit.html.twig
        created: templates/product/index.html.twig
        created: templates/product/new.html.twig
        created: templates/product/show.html.twig



## Managing Errors and 404 Pages


    use Symfony\Component\HttpKernel\Exception\NotFoundHttpException;

    // ...
    public function index()
    {
        // retrieve the object from database
        $product = ...;
        if (!$product) {
            throw $this->createNotFoundException('The product does not exist');

            // the above is just a shortcut for:
            // throw new NotFoundHttpException('The product does not exist');
        }

        return $this->render(...);
    }

    // this exception ultimately generates a 500 status error
    throw new \Exception('Something went wrong!');

## The Request object as a Controller Argument

    use Symfony\Component\HttpFoundation\Request;

    public function index(Request $request, $firstName, $lastName)
    {
        $page = $request->query->get('page', 1);

        // ...
    }

[More infos on request object](https://symfony.com/doc/current/controller.html#request-object-info)

## Managing the Session


Symfony provides a session service that you can use to store information about the user between requests. 
Session is enabled by default, but will only be started if you read or write from it.
Session storage and other configuration can be controlled under the 
framework.session configuration in config/packages/framework.yaml.
To get the session, add an argument and type-hint it with SessionInterface:

    use Symfony\Component\HttpFoundation\Session\SessionInterface;

    public function index(SessionInterface $session)
    {
        // stores an attribute for reuse during a later user request
        $session->set('foo', 'bar');

        // gets the attribute set by another controller in another request
        $foobar = $session->get('foobar');

        // uses a default value if the attribute doesn't exist
        $filters = $session->get('filters', []);
    }

For more info, [see Sessions](https://symfony.com/doc/current/session.html).

### Flash Messages

[example with form](https://symfony.com/doc/current/forms.html)

use Symfony\Component\HttpFoundation\Request;

    public function update(Request $request)
    {
        // ...

        if ($form->isSubmitted() && $form->isValid()) {
            // do some sort of processing

            $this->addFlash(
                'notice',
                'Your changes were saved!'
            );
            // $this->addFlash() is equivalent to $request->getSession()->getFlashBag()->add()

            return $this->redirectToRoute(...);
        }

        return $this->render(...);
    }

    {# templates/base.html.twig #}

    {# read and display just one flash message type #}
    {% for message in app.flashes('notice') %}
        <div class="flash-notice">
            {{ message }}
        </div>
    {% endfor %}

    {# read and display several types of flash messages #}
    {% for label, messages in app.flashes(['success', 'warning']) %}
        {% for message in messages %}
            <div class="flash-{{ label }}">
                {{ message }}
            </div>
        {% endfor %}
    {% endfor %}

    {# read and display all flash messages #}
    {% for label, messages in app.flashes %}
        {% for message in messages %}
            <div class="flash-{{ label }}">
                {{ message }}
            </div>
        {% endfor %}
    {% endfor %}

### The Request and Response Object

Symfony will pass the Request object to any controller argument that is type-hinted with the Request class:

    use Symfony\Component\HttpFoundation\Request;

    public function index(Request $request)
    {
        $request->isXmlHttpRequest(); // is it an Ajax request?

        $request->getPreferredLanguage(['en', 'fr']);

        // retrieves GET and POST variables respectively
        $request->query->get('page');
        $request->request->get('page');

        // retrieves SERVER variables
        $request->server->get('HTTP_HOST');

        // retrieves an instance of UploadedFile identified by foo
        $request->files->get('foo');

        // retrieves a COOKIE value
        $request->cookies->get('PHPSESSID');

        // retrieves an HTTP request header, with normalized, lowercase keys
        $request->headers->get('host');
        $request->headers->get('content-type');
    }

In Symfony, a controller is required to return a Response object:

    use Symfony\Component\HttpFoundation\Response;

    // creates a simple Response with a 200 status code (the default)
    $response = new Response('Hello '.$name, Response::HTTP_OK);

    // creates a CSS-response with a 200 status code
    $response = new Response('<style> ... </style>');
    $response->headers->set('Content-Type', 'text/css');


### Accessing Configuration Values

    // ...
    public function index()
    {
        $contentsDir = $this->getParameter('kernel.project_dir').'/contents';
        // ...
    }


### Returning JSON Response

    // ...
    public function index()
    {
        // returns '{"username":"jane.doe"}' and sets the proper Content-Type header
        return $this->json(['username' => 'jane.doe']);

        // the shortcut defines three optional arguments
        // return $this->json($data, $status = 200, $headers = [], $context = []);
    }


### Streaming File Responses

You can use the file() helper to serve a file from inside a controller :

    public function download()
    {
        // send the file contents and force the browser to download it
        return $this->file('/path/to/some_file.pdf');
    }

The file() helper provides some arguments to configure its behavior:
    
    use Symfony\Component\HttpFoundation\File\File;
    use Symfony\Component\HttpFoundation\ResponseHeaderBag;

    public function download()
    {
        // load the file from the filesystem
        $file = new File('/path/to/some_file.pdf');

        return $this->file($file);

        // rename the downloaded file
        return $this->file($file, 'custom_name.pdf');

        // display the file contents in the browser instead of downloading it
        return $this->file('invoice_3241.pdf', 'my_invoice.pdf', ResponseHeaderBag::DISPOSITION_INLINE);
    }

## Final Thoughts

In Symfony, a controller is usually a class method which is used to accept requests, 
and return a Response object. When mapped with a URL, 
a controller becomes accessible and its response can be viewed.

To facilitate the development of controllers, 
Symfony provides an AbstractController. 
It can be used to extend the controller class allowing access to some 
frequently used utilities such as render() and redirectToRoute(). 
The AbstractController also provides the createNotFoundException() 
utility which is used to return a page not found response.

In other articles, you'll learn how to use specific services 
from inside your controller that will help you persist and 
fetch objects from a database, process form submissions, 
handle caching and more.

## Learn more about Controllers¶

* [Extending Action Argument Resolving](https://symfony.com/doc/current/controller/argument_value_resolver.html)
* [How to Customize Error Pages](https://symfony.com/doc/current/controller.html)
* [How to Forward Requests to another Controller](https://symfony.com/doc/current/controller/forwarding.html)
* [How to Define Controllers as Services](https://symfony.com/doc/current/controller/service.html)
* [How to Create a SOAP Web Service in a Symfony Controller](https://symfony.com/doc/current/controller/soap_web_service.html)
* [How to Upload Files](https://symfony.com/doc/current/controller/upload_file.html)
