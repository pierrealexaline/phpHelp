# Routes with Symfony

from [Routing](https://symfony.com/doc/current/routing.html)


* Creating Routes
 * Creating Routes as Annotations
 * Creating Routes in YAML, XML or PHP Files
 * Matching HTTP Methods
 * Matching Expressions
 * Debugging Routes
* Route Parameters
 * Parameters Validation
 * Optional Parameters
 * Parameter Conversion
 * Special Parameters
 * Extra Parameters
 * Slash Characters in Route Parameters
* Route Groups and Prefixes
* Getting the Route Name and Parameters
* Special Routes
 * Rendering a Template Directly from a Route
 * Redirecting to URLs and Routes Directly from a Route
* Sub-Domain Routing
* Localized Routes (i18n)
* Generating URLs
 * Generating URLs in Controllers
 * Generating URLs in Services
 * Generating URLs in Templates
 * Generating URLs in JavaScript
 * Generating URLs in Commands
 * Checking if a Route Exists
 * Forcing HTTPS on Generated URLs
* Troubleshooting
* Learn more about Routing

## Creating Routes as Annotations

    // src/Controller/BlogController.php
    namespace App\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
    use Symfony\Component\Routing\Annotation\Route;

    class BlogController extends AbstractController
    {
        /**
        * @Route("/blog", name="blog_list")
        */
        public function list()
        {
            // ...
        }
    }

## Matching HTTP Methods - example API usages

    // src/Controller/BlogApiController.php
    namespace App\Controller;

    // ...

    class BlogApiController extends AbstractController
    {
        /**
        * @Route("/api/posts/{id}", methods={"GET","HEAD"})
        */
        public function show(int $id)
        {
            // ... return a JSON response with the post
        }

        /**
        * @Route("/api/posts/{id}", methods={"PUT"})
        */
        public function edit(int $id)
        {
            // ... edit a post
        }
    }


## Matching Expressions with condition option


    // src/Controller/DefaultController.php
    namespace App\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
    use Symfony\Component\Routing\Annotation\Route;

    class DefaultController extends AbstractController
    {
        /**
        * @Route(
        *     "/contact",
        *     name="contact",
        *     condition="context.getMethod() in ['GET', 'HEAD'] and request.headers.get('User-Agent') matches '/firefox/i'"
        * )
        *
        * expressions can also include config parameters:
        * condition: "request.headers.get('User-Agent') matches '%app.allowed_browsers%'"
        */
        public function contact()
        {
            // ...
        }
    }


## Debugging Routes

    php bin/console debug:router
    php bin/console debug:router app_lucky_number
    php bin/console router:match /lucky/number/8


## Route Parameters

    // src/Controller/BlogController.php
    namespace App\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
    use Symfony\Component\Routing\Annotation\Route;

    class BlogController extends AbstractController
    {
        // ...

        /**
        * @Route("/blog/{slug}", name="blog_show")
        */
        public function show(string $slug)
        {
            // $slug will equal the dynamic part of the URL
            // e.g. at /blog/yay-routing, then $slug='yay-routing'

            // ...
        }


### Parameters Validation

    // src/Controller/BlogController.php
    namespace App\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
    use Symfony\Component\Routing\Annotation\Route;

    class BlogController extends AbstractController
    {
        /**
        * @Route("/blog/{page}", name="blog_list", requirements={"page"="\d+"})
        */
        public function list(int $page)
        {
            // ...
        }

        /**
        * @Route("/blog/{slug}", name="blog_show")
        */
        public function show($slug)
        {
            // ...
        }
    }

    // src/Controller/BlogController.php
    namespace App\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
    use Symfony\Component\Routing\Annotation\Route;

    class BlogController extends AbstractController
    {
        /**
        * @Route("/blog/{page<\d+>}", name="blog_list")
        */
        public function list(int $page)
        {
            // ...
        }
    }


### Optional Parameters


    // src/Controller/BlogController.php
    namespace App\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
    use Symfony\Component\Routing\Annotation\Route;

    class BlogController extends AbstractController
    {
        /**
        * @Route("/blog/{page}", name="blog_list", requirements={"page"="\d+"})
        */
        public function list(int $page = 1)
        {
            // ...
        }
    }


    // src/Controller/BlogController.php
    namespace App\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
    use Symfony\Component\Routing\Annotation\Route;

    class BlogController extends AbstractController
    {
        /**
        * @Route("/blog/{page<\d+>?1}", name="blog_list")
        */
        public function list(int $page)
        {
            // ...
        }
    }


### Parameter Conversion


    composer require annotations

    // src/Controller/BlogController.php
    namespace App\Controller;

    use App\Entity\BlogPost;
    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
    use Symfony\Component\Routing\Annotation\Route;

    class BlogController extends AbstractController
    {
        // ...

        /**
        * @Route("/blog/{slug}", name="blog_show")
        */
        public function show(BlogPost $post)
        {
            // $post is the object whose slug matches the routing parameter

            // ...
        }
    }


### Special Parameters

    // src/Controller/ArticleController.php
    namespace App\Controller;

    // ...
    class ArticleController extends AbstractController
    {
        /**
        * @Route(
        *     "/articles/{_locale}/search.{_format}",
        *     locale="en",
        *     format="html",
        *     requirements={
        *         "_locale": "en|fr",
        *         "_format": "html|xml",
        *     }
        * )
        */
        public function search()
        {
        }
    }


### Extra Parameters

    // src/Controller/BlogController.php
    namespace App\Controller;

    use Symfony\Component\Routing\Annotation\Route;

    class BlogController
    {
        /**
        * @Route("/blog/{page}", name="blog_index", defaults={"page": 1, "title": "Hello world!"})
        */
        public function index(int $page, string $title)
        {
            // ...
        }
    }

### Slash Characters in Route Parameters

    // src/Controller/DefaultController.php
    namespace App\Controller;

    use Symfony\Component\Routing\Annotation\Route;

    class DefaultController
    {
        /**
        * @Route("/share/{token}", name="share", requirements={"token"=".+"})
        */
        public function share($token)
        {
            // ...
        }
    }

## Route Groups and Prefixes

    // src/Controller/BlogController.php
    namespace App\Controller;

    use Symfony\Component\Routing\Annotation\Route;

    /**
     * @Route("/blog", requirements={"_locale": "en|es|fr"}, name="blog_")
     */
    class BlogController
    {
        /**
         * @Route("/{_locale}", name="index")
         */
        public function index()
        {
            // ...
        }

        /**
         * @Route("/{_locale}/posts/{slug}", name="show")
         */
        public function show(Post $post)
        {
            // ...
        }
    }
    

## Getting the Route Name and Parameters

    // src/Controller/BlogController.php
    namespace App\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
    use Symfony\Component\HttpFoundation\Request;
    use Symfony\Component\Routing\Annotation\Route;

    class BlogController extends AbstractController
    {
        /**
        * @Route("/blog", name="blog_list")
        */
        public function list(Request $request)
        {
            // ...

            $routeName = $request->attributes->get('_route');
            $routeParameters = $request->attributes->get('_route_params');

            // use this to get all the available attributes (not only routing ones):
            $allAttributes = $request->attributes->all();
        }
    }


    {% set route_name = app.request.attributes.get('_route') %}
    {% set route_parameters = app.request.attributes.get('_route_params') %}

    {# use this to get all the available attributes (not only routing ones) #}
    {% set all_attributes = app.request.attributes.all %}

## Special Routes


### Rendering a Template Directly from a Route
[Rendering a template directly from a route](https://symfony.com/doc/current/templates.html#templates-render-from-route)


### Redirecting to URLs and Routes Directly from a Route


    // config/routes.php
    use App\Controller\DefaultController;
    use Symfony\Bundle\FrameworkBundle\Controller\RedirectController;
    use Symfony\Component\Routing\Loader\Configurator\RoutingConfigurator;

    return function (RoutingConfigurator $routes) {
        $routes->add('doc_shortcut', '/doc')
            ->controller(RedirectController::class)
            ->defaults([
                'route' => 'doc_page',
                // optionally you can define some arguments passed to the template
                'page' => 'index',
                'version' => 'current',
                // redirections are temporary by default (code 302) but you can make them permanent (code 301)
                'permanent' => true,
                // add this to keep the original query string parameters when redirecting
                'keepQueryParams' => true,
                // add this to keep the HTTP method when redirecting. The redirect status changes:
                // * for temporary redirects, it uses the 307 status code instead of 302
                // * for permanent redirects, it uses the 308 status code instead of 301
                'keepRequestMethod' => true,
            ])
        ;

        $routes->add('legacy_doc', '/legacy/doc')
            ->controller(RedirectController::class)
            ->defaults([
                // this value can be an absolute path or an absolute URL
                'path' => 'https://legacy.example.com/doc',
                // redirections are temporary by default (code 302) but you can make them permanent (code 301)
                'permanent' => true,
            ])
        ;
    };



### Redirecting URLs with Trailing Slashes

Route URL 	If the requested URL is /foo 	If the requested URL is /foo/
/foo 	It matches (200 status response) 	It makes a 301 redirect to /foo
/foo/ 	It makes a 301 redirect to /foo/ 	It matches (200 status response)


## Sub-Domain Routing


    // config/routes.php
    use App\Controller\MainController;
    use Symfony\Component\Routing\Loader\Configurator\RoutingConfigurator;

    return function (RoutingConfigurator $routes) {
        $routes->add('mobile_homepage', '/')
            ->controller([MainController::class, 'mobileHomepage'])
            ->host('m.example.com')
        ;
        $routes->add('homepage', '/')
            ->controller([MainController::class, 'homepage'])
        ;
    };
    return $routes;


    // config/routes.php
    use App\Controller\MainController;
    use Symfony\Component\Routing\Loader\Configurator\RoutingConfigurator;

    return function (RoutingConfigurator $routes) {
        $routes->add('mobile_homepage', '/')
            ->controller([MainController::class, 'mobileHomepage'])
            ->host('{subdomain}.example.com')
            ->defaults([
                'subdomain' => 'm',
            ])
            ->requirements([
                'subdomain' => 'm|mobile',
            ])
        ;
        $routes->add('homepage', '/')
            ->controller([MainController::class, 'homepage'])
        ;
    };

## Localized Routes (i18n)

    // config/routes.php
    use App\Controller\CompanyController;
    use Symfony\Component\Routing\Loader\Configurator\RoutingConfigurator;

    return function (RoutingConfigurator $routes) {
        $routes->add('about_us', [
            'en' => '/about-us',
            'nl' => '/over-ons',
        ])
            ->controller([CompanyController::class, 'about'])
        ;
    };

    // config/routes/annotations.php
    use Symfony\Component\Routing\Loader\Configurator\RoutingConfigurator;

    return function (RoutingConfigurator $routes) {
        $routes->import('../src/Controller/', 'annotation')
            ->prefix([
                // don't prefix URLs for English, the default locale
                'en' => '',
                'nl' => '/nl'
            ])
        ;
    };

    // src/Controller/BlogController.php
    namespace App\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
    use Symfony\Component\Routing\Annotation\Route;
    use Symfony\Component\Routing\Generator\UrlGeneratorInterface;

    class BlogController extends AbstractController
    {
        /**
        * @Route("/blog", name="blog_list")
        */
        public function list()
        {
            // ...

            // generate a URL with no route arguments
            $signUpPage = $this->generateUrl('sign_up');

            // generate a URL with route arguments
            $userProfilePage = $this->generateUrl('user_profile', [
                'username' => $user->getUsername(),
            ]);

            // generated URLs are "absolute paths" by default. Pass a third optional
            // argument to generate different URLs (e.g. an "absolute URL")
            $signUpPage = $this->generateUrl('sign_up', [], UrlGeneratorInterface::ABSOLUTE_URL);

            // when a route is localized, Symfony uses by default the current request locale
            // pass a different '_locale' value if you want to set the locale explicitly
            $signUpPageInDutch = $this->generateUrl('sign_up', ['_locale' => 'nl']);
        }
    }

    $this->generateUrl('blog', ['page' => 2, 'category' => 'Symfony']);
    // the 'blog' route only defines the 'page' parameter; the generated URL is:
    // /blog/2?category=Symfony

## Generating URLs in Services

    // src/Service/SomeService.php
    namespace App\Service;

    use Symfony\Component\Routing\Generator\UrlGeneratorInterface;

    class SomeService
    {
        private $router;

        public function __construct(UrlGeneratorInterface $router)
        {
            $this->router = $router;
        }

        public function someMethod()
        {
            // ...

            // generate a URL with no route arguments
            $signUpPage = $this->router->generate('sign_up');

            // generate a URL with route arguments
            $userProfilePage = $this->router->generate('user_profile', [
                'username' => $user->getUsername(),
            ]);

            // generated URLs are "absolute paths" by default. Pass a third optional
            // argument to generate different URLs (e.g. an "absolute URL")
            $signUpPage = $this->router->generate('sign_up', [], UrlGeneratorInterface::ABSOLUTE_URL);

            // when a route is localized, Symfony uses by default the current request locale
            // pass a different '_locale' value if you want to set the locale explicitly
            $signUpPageInDutch = $this->router->generate('sign_up', ['_locale' => 'nl']);
        }
    }


## Generating URLs in Templates

[Urls generating in template](https://symfony.com/doc/current/templates.html#templates-link-to-pages)

## Generating URLs in JavaScript

    <script>
        const route = "{{ path('blog_show', {slug: 'my-blog-post'})|escape('js') }}";
    </script>

## Generating URLs in Commands

    // config/services.php
    $container->setParameter('router.request_context.host', 'example.org');
    $container->setParameter('router.request_context.base_url', 'my/path');
    $container->setParameter('asset.request_context.base_path', $container->getParameter('router.request_context.base_url'));

    // src/Command/SomeCommand.php
    namespace App\Command;

    use Symfony\Component\Routing\Generator\UrlGeneratorInterface;
    use Symfony\Component\Routing\RouterInterface;
    // ...

    class SomeCommand extends Command
    {
        private $router;

        public function __construct(RouterInterface $router)
        {
            parent::__construct();

            $this->router = $router;
        }

        protected function execute(InputInterface $input, OutputInterface $output)
        {
            // these values override any global configuration
            $context = $this->router->getContext();
            $context->setHost('example.com');
            $context->setBaseUrl('my/path');

            // generate a URL with no route arguments
            $signUpPage = $this->router->generate('sign_up');

            // generate a URL with route arguments
            $userProfilePage = $this->router->generate('user_profile', [
                'username' => $user->getUsername(),
            ]);

            // generated URLs are "absolute paths" by default. Pass a third optional
            // argument to generate different URLs (e.g. an "absolute URL")
            $signUpPage = $this->router->generate('sign_up', [], UrlGeneratorInterface::ABSOLUTE_URL);

            // when a route is localized, Symfony uses by default the current request locale
            // pass a different '_locale' value if you want to set the locale explicitly
            $signUpPageInDutch = $this->router->generate('sign_up', ['_locale' => 'nl']);

            // ...
        }
    }

## Checking if a Route Exists

    use Symfony\Component\Routing\Exception\RouteNotFoundException;

    // ...
    try {
        $url = $this->router->generate($routeName, $routeParameters);
    } catch (RouteNotFoundException $e) {
        // the route is not defined...
    }

## Forcing HTTPS on Generated URLs

    // config/services.php
    $container->setParameter('router.request_context.scheme', 'https');
    $container->setParameter('asset.request_context.secure', true);

    // config/routes.php
    use App\Controller\SecurityController;
    use Symfony\Component\Routing\Loader\Configurator\RoutingConfigurator;

    return function (RoutingConfigurator $routes) {
        $routes->add('login', '/login')
            ->controller([SecurityController::class, 'login'])
            ->schemes(['https'])
        ;
    };

    {# if the current scheme is HTTPS, generates a relative URL: /login #}
    {{ path('login') }}

    {# if the current scheme is HTTP, generates an absolute URL to change
    the scheme: https://example.com/login #}
    {{ path('login') }}


    // config/routes/annotations.php
    use Symfony\Component\Routing\Loader\Configurator\RoutingConfigurator;

    return function (RoutingConfigurator $routes) {
        $routes->import('../src/Controller/', 'annotation')
            ->schemes(['https'])
        ;
    };

The Security component provides another way to enforce HTTP or HTTPS via the requires_channel setting.

## Troubleshooting


    Controller "App\Controller\BlogController::show()" requires that you provide a value for the "$slug" argument.

This happens when your controller method has an argument (e.g. $slug):

    public function show($slug)
    {
        // ...
    }

But your route path does not have a {slug} parameter (e.g. it is /blog/show). Add a {slug} to your route path: /blog/show/{slug} or give the argument a default value (i.e. $slug = null).

    Some mandatory parameters are missing ("slug") to generate a URL for route "blog_show".

This means that you're trying to generate a URL to the blog_show route but you are not passing a slug value (which is required, because it has a {slug} parameter in the route path). To fix this, pass a slug value when generating the route:

    $this->generateUrl('blog_show', ['slug' => 'slug-value']);

    // or, in Twig
    // {{ path('blog_show', {slug: 'slug-value'}) }}


[How to Create a custom Route Loader](https://symfony.com/doc/current/routing/custom_route_loader.html) 
(Looking up Routes from a Database: Symfony CMF DynamicRouter)[https://symfony.com/doc/current/routing/routing_from_database.html]