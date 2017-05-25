This repo is a set of notes I write for myself preparing for the Symfony Certification. Maybe it
helps other people too, but I'd like to point a few things out:

- the notes are primarily for myself, I do not have educational standards.
- all information herein is publicly available, either from links I provide or from my experience; 
  there are no secrets of the certification process, simply because I don't know any (and will 
  not share them after I took the certification).
- there will be errors. Do not rely too heavily on this repo; pull requests with fixes are welcome
  though (but only if they do not reveal "insider knowledge" from the certification).
- this repo is completely independent from SensioLabs.

The chapters in this file follow the topic list from SensioLabs: https://sensiolabs.com/en/symfony/certification.html
They contain mostly short bullet points for a quick overview and cannot replace own research and
experience.


PHP and Web Security
====================

PHP 5.3 to PHP 5.6 API
----------------------

- Significant changes in PHP 5.4:
  - Traits.
  - Short array syntax.
  - Function array dereferencing (`foo()[0]`).
  - $this can be used in Closures.
  - Class members can be accessed directly after instantiation (`(new Foo())->bar()`).
  - Binary numbers, e.g. 0b10101010.
  - Built-in development web server.
  - `callable` type hint.
  - Register globals, magic quotes, safe mode removed.
- Significant changes in PHP 5.5:
  - Generators.
  - `finally` keyword.
  - New password hashing API (`password_hash()`, `password_verify()`).
  - `::class` keyword for fully qualified class name resolution (performed at compile time, without autoloading).
  - Expressions allowed as argument for `empty()`.
  - Arrays of arrays can be unpacked with `list()` in a `foreach` loop, e.g. 
    `foreach ($arrayOfArrays as list($internalElement1, $internalElement2))`.
  - OPCache extension.
  - No more support for Windows XP and 2003.
- Significant changes in PHP 5.6:
  - Constant scalar expressions (expressions in places where in former versions only static values were allowed, e.g.
    in constants, function argument default values, class property declarations).
  - Variadic functions via `...` (before, it was only possible to get all arguments via `func_get_args()` if more
    arguments were expected than declared).
  - Argument unpacking via `...` (arrays and traversables can be converted to argument lists).
  - `**` exponentiation operator.
  - Constant and function imports via `use`.
  - Default character set for some functions (`htmlentities()`, `html_entity_decode()`, `htmlspecialchars()`) is UTF-8.
  - SSL/TLS peer verification on by default.

https://secure.php.net/releases/5_4_0.php

https://secure.php.net/releases/5_5_0.php

https://secure.php.net/releases/5_6_0.php

Object Oriented Programming
---------------------------

Namespaces
----------

Interfaces
----------

Anonymous functions and closures
--------------------------------

- Anonymous function: Function without a name, used inline as callback.
- Closure: Function assigned to a variable, can be executed by accessing the variable and adding brackets + arguments.

https://en.wikipedia.org/wiki/Closure_(computer_programming)

https://secure.php.net/manual/en/functions.anonymous.php

Abstract classes
----------------

- Class modifier "abstract".
- Abstract methods (semicolon instead of method body) can be defined.
- Cannot be instantiated directly, needs to be subclassed.
- Concrete subclasses must define abstract methods.
- Used as basic implementation, often to define a common workflow (e.g. template method pattern).

Exception and error handling
----------------------------

- Traditionally errors are raised as notices, warnings, errors and fatal errors.
- Those PHP errors are output to the browser and logged in the web server logs, both of which can be 
  disabled or limited to certain error types ("error_reporting" and "display_errors" - the latter 
  can only be completely enabled or disabled; both can be set in php.ini and using ini_set()).
- Error output can be suppressed by adding an @ before a function or method call (though this is
  generally discouraged).
- Errors cannot normally be treated in another way than logging and displaying; however, it is
  possible to register a custom error handler function using set_error_handler('functionName').
- A custom error handler receives information on the error type, the message, the file, the line
  number and a context.
- Setting error_reporting so that e.g. a notice will not be reported means that a custom error handler
  is not triggered when a notice would be raised.
  
- Exceptions are more common in object-oriented programming languages.
- Exceptions are themselves classes that derive from \Exception.
- Exceptions can be thrown by built-in and user code.
- When an exception is thrown, the current method execution is aborted and the exception object
  bubbles up the execution path until they are either caught in a method on a higher level, or 
  the outermost method call is reached. In the latter case, the thread is aborted and a stack trace
  is rendered (output can be disabled in PHP with display_errors).

https://secure.php.net/manual/en/errorfunc.configuration.php#ini.error-reporting

https://secure.php.net/manual/en/errorfunc.configuration.php#ini.display-errors

https://secure.php.net/manual/en/function.set-error-handler.php

Traits
------

- Traits are additions to classes that can define methods and properties.
- A class can "use" a trait by adding the "use" keyword. The properties and methods of the trait
  will then behave as if copy-and-pasted into the class definition. This is a workaround for the
  fact that in PHP no multi-inheritance is allowed.
- If a class defines a method with the same name as a method in a trait, the class method overrides
  the one in the trait. However, a trait method overrides a method with the same name of a super
  class.
- A class can use multiple traits. If method names conflict, one of the methods must be selected
  using the "insteadof" keyword (e.g. B::foo instead of A if A and B are classes and foo is a method
  defined in both of them).
- An alias can be given to a method by using the "as" keyword (e.g. B::foo as bar). The method can
  then be called by its alias. In a naming conflict this makes it possible to use both methods by
  providing an alias for one of them.
- See link below for more features such as changing method visibility, traits composed from traits,
  abstract and static members.
- As traits can contain properties and not only methods, PHP traits are really mixins.
  
https://secure.php.net/manual/en/language.oop5.traits.php

PHP extensions
--------------

SPL
---

Web security (XSS, CSRF, etc.)
------------------------------

HTTP
====

Client / Server interaction
---------------------------

Status codes
------------

- Every HTTP response contains a 3-digit status code indicating various outcomes of the request.
  The most important status codes are:
  - 200 everything went as expected
  - 301 permanent redirect
  - 302 found (often used as non-permanent redirect, contradicting the standard)
  - 403 access denied
  - 404 file/resource not found
  - 500 internal server error
  
https://en.wikipedia.org/wiki/List_of_HTTP_status_codes

HTTP request
------------

HTTP response
-------------

HTTP methods
------------

Cookies
-------

Caching
-------

Content negotiation
-------------------

Language detection
------------------

- The client may pass the HTTP header "Accept-Language" which contains an ordered list of accepted languages.
- The server is not bound to this header but may choose the served language in another way.
- It is common that URLs contain a language identifier (often the 2-letter ISO 639-1 identifier) to
  specify the resource's language. This way, a resource that is available in multiple languages is
  unambiguously identifiable by an URL.
- Symfony: The JMSI18nRoutingBundle provides support for internationalized routes.

https://github.com/schmittjoh/JMSI18nRoutingBundle

Symfony Architecture
====================

Symfony Standard Edition
------------------------

- The Symfony Standard Edition is an application skeleton that aids in starting a new application.
- It contains the full-stack Symfony FrameworkBundle as well as some third-party bundles and their
  configuration.
- The Symfony Standard Edition as well as editions as a whole will be discontinued and replaced by
  Symfony Flex in Symfony 4.0 (not part of this certification).

https://symfony.com/doc/current/setup.html

https://github.com/symfony/symfony-standard

http://fabien.potencier.org/symfony4-compose-applications.html

License
-------

- Symfony is licensed under the MIT license.
- The MIT license is a permissive license that imposes almost no limitations on what users are
  allowed to do.
- This license is also very common in the whole Symfony eco-system.
- When contributing to Symfony, the author must publish the contributions under the MIT license.

https://github.com/symfony/symfony/blob/master/LICENSE

https://symfony.com/doc/current/contributing/code/patches.html#make-a-pull-request

Components
----------

Symfony is a set of reusable components. Each component can be used individually, but it is also
possible to use all of them together as a full-stack framework.

As of Symfony 3.0 these components are available:

- Asset
- BrowserKit
- ClassLoader (deprecated as of 3.3 in favor of Composer)
- Config
- Console
- CssSelector
- Debug
- DependencyInjection
- DomCrawler
- EventDispatcher
- ExpressionLanguage
- Filesystem
- Finder
- Form
- HttpFoundation
- HttpKernel
- Inflector (Symfony-internal use only)
- Intl
- LDAP
- OptionsResolver
- Process
- PropertyAccess
- PropertyInfo
- Routing
- Security
- Serializer
- Stopwatch
- Templating
- Translation
- Validation
- VarDumper
- YAML

In later Symfony versions, more components were added

- Cache (3.2+)
- Lock (3.3+)
- WebLink (3.3+)
- Workflow (3.2+)

https://symfony.com/doc/current/components/index.html

Bundles
-------

Symfony comes with some bundles:

- DebugBundle
- FrameworkBundle
- SecurityBundle
- TwigBundle
- WebProfilerBundle

https://symfony.com/doc/bundles/

Bridges
-------

Symfony comes with some bridges:

- Doctrine Bridge
- Monolog Bridge
- PhpUnit Bridge
- ProxyManager Bridge
- Twig Bridge

These bridges integrate external libraries with Symfony.

Configuration
-------------

There are basically two ways to configure an application and its bundles:

- container parameters
- semantic configuration

https://symfony.com/doc/current/configuration.html
https://symfony.com/doc/current/configuration/configuration_organization.html
https://symfony.com/doc/current/bundles/configuration.html

Code organization
-----------------

Request handling
----------------

Exception handling
------------------

- Exceptions that are not handled by the application will be caught by Symfony's HttpKernel class
  (or the Application class if running a console command).
- A kernel.exception event will be dispatched, and event listeners can handle the exception.
- Event listeners can also set an HTTP status code. If this is not done, the status code is retrieved
  from the exception if it is an instance of HttpExceptionInterface. Otherwise status 500 is set.
- In dev mode, an exception page showing the stack trace will be displayed if no custom exception
  handler intervenes.
- In prod mode, a simpler error page will be displayed in order not to leak internals.

Event dispatcher and kernel events
----------------------------------

- The event dispatcher is a Symfony component that publishes events to subscribers.
- There are event listeners and event subscribers. The former are wired to the events they listen to
  in the dependency injection container, the latter define the events they listen to themselves.
- Event listeners and subscribers are normally defined in the dependency injection container by
  service definitions that contain the tag kernel.event_listener or kernel.event_subscribe.
- It is also possible to add event listeners at runtime.
- There are some predefined events, but custom events can be defined by simply dispatching them.
  Events are only identified by their name.
- An event can also have a payload which is encapsulated in the class Symfony\Component\EventDispatcher\Event.
- It is possible to define custom events by subclassing the Event class.
- Event listeners can be given a priority to change the order in which they are called.
- The kernel dispatches a few events during the request's lifecycle. These are:
  - kernel.request at the beginning of the request. Event listeners may provide a response in
    which case the request takes a "shortcut" (only the kernel.response, kernel.finish_request
    and kernel.terminate events are dispatched). Typically routing and access checks are performed
    here.
  - kernel.controller is dispatched after kernel.request set controller options and a controller
    for the request was resolved. Using this event, the controller can be changed or modified.
  - kernel.view is dispatched after the controller was called but did not return a Response object.
    Listeners should generate a Response object from the controller's response or an exception is thrown.
  - kernel.response is dispatched after the response is ultimately known. Listeners may change or
    replace the response.
  - kernel.terminate is dispatched at the very end of the request. Depending on the PHP interpreter,
    the connection will be closed at that time and the client will not be influenced by longer-running
    actions. This event can e.g. be used to send emails without slowing down the request.
  
https://symfony.com/doc/current/event_dispatcher.html

Official best practices
-----------------------

The official best practices are a set of suggestions to keep an application simple.

https://symfony.com/doc/current/best_practices/index.html

Release management
------------------

- A new minor version of Symfony is released every 6 months (May and November).
- A new major version of Symfony is released every two years (November).
- There is a development phase lasting 4 months and a stabilization phase lasting 2 months.
- The stabilization phase allows the Symfony eco-system to adjust to the new version.
- There are 5 minor versions for each major version, with the last minor version being an LTS
  release.
- A standard version is supported for 8 months after release, and receives security fixes
  for 14 months after release.
- An LTS release is supported for 3 years after release, and receives security fixes for
  4 years after release.

https://symfony.com/doc/current/contributing/community/releases.html

Backward compatibility promise
------------------------------

- Symfony uses Semantic Versioning. Only major releases are allowed to break backward compatibility.
- Minor and fix releases must ensure that the existing API is retained.
- It is also possible to add a new way to do things as long as the old way stays the same (until
  the next major version).
- BC is not assured if user code does one of the following:
  - Use an interface that is marked as @internal.
  - Extend a class and add a property or method.
  - Call a private property or method via reflection.
  - Use or modify code in a namespace containing "Tests"

https://symfony.com/doc/current/contributing/code/bc.html

Deprecations best practices
---------------------------

Following semantic versioning, deprecations are only allowed in major or minor versions, not in fix versions.

https://symfony.com/doc/current/contributing/code/conventions.html#contributing-code-conventions-deprecations

Standardization
===============

Release management and roadmap schedule
---------------------------------------

See above.

Framework interoperability and PSRs
-----------------------------------

Symfony embraces standards and interoperability. Symfony components can be used in other frameworks
and Symfony itself uses some standard components like Doctrine and Composer instead of reinventing
all wheels. Symfony is member of the PHP-FIG group that standardizes common PHP tasks.

Symfony implements the following PSR standards:

- PSR-0 (basic class autoloading, deprecated)
- PSR-1 (coding style)
- PSR-2 (coding style)
- PSR-3 (logger interface)
- PSR-4 (class autoloading, Symfony class loader deprecated in 3.3)
- PSR-6 (caching interface, Symfony 3.1+)
- PSR-7 (HTTP message interface)
- PSR-11 (service container interface, Symfony 3.3+, standard not accepted yet)
- PSR-13 (hypermedia links, Symfony 3.3+)
- PSR-16 (simple cache, Symfony 3.3+)

So Symfony supports all PSRs as of version 3.3. In Symfony 3.0 PSRs 6, 11, 13, 16 are not yet 
supported.

http://www.php-fig.org/psr/

Naming conventions
------------------

https://symfony.com/doc/current/contributing/code/conventions.html

Coding standards
----------------

In short: Follow PSR-0, -1, -2, -4 and some extra rules from the first link below.
 
There is a code style fixer available that formats code automatically. This tool allows
to configure which changes should be applied, and it can be configured so that it automatically
follows the Symfony coding standards.

When contributing to Symfony, the fabbot will check the submitted code for code style issues
(among other checks).

https://symfony.com/doc/current/contributing/code/standards.html
https://github.com/FriendsOfPhp/PHP-CS-Fixer

Third-party libraries integration
---------------------------------

The Symfony standard edition integrates the following third-party libraries:

- Doctrine ORM (which brings the Doctrine DBAL lib)
- DoctrineBundle
- DoctrineCacheBundle
- Incenteev ComposerParameterHandler (interactive generation of the parameters.yml file)
- Monolog (including the MonologBundle for Symfony integration)
- paragonie/random-compat (compatibility layer for random number generation unification across PHP versions)
- SensioDistributionBundle (composer hooks for some post-install/post-update tasks, security checker)
- SensioFrameworkExtraBundle (annotation support for controllers, PSR-7 support for injection into controllers)
- SensioGeneratorBundle (commands for code generation for bundles, commands, controllers, forms, Doctrine)
- Swiftmailer (including the Swiftmailer bundle for Symfony integration)
- Twig

Composer packages handling
--------------------------

Development best practices
--------------------------

Framework overloading
---------------------

Semantic versioning
-------------------

From semver.org:

---

Given a version number MAJOR.MINOR.PATCH, increment the:

- MAJOR version when you make incompatible API changes,
- MINOR version when you add functionality in a backwards-compatible manner, and
- PATCH version when you make backwards-compatible bug fixes.

Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

---

The statements above apply only to the public API of a library or framework. It is up to the
developer to define what is public.

Semantic versioning gives a user of a library or framework the confidence that upgrading to a new
minor or patch version will not break existing functionality.

http://semver.org/
    
Bundles
=======

Naming conventions
------------------

A bundle should always be named with a "Bundle" suffix. The bundle name should be rather short
and as most identifiers in the Symfony eco-system be written in camel-case.

Example: HelloWorldBundle.

A bundle that is intended for reuse also has the vendor name as prefix for the bundle.

Example: AcmeHelloWorldBundle.

A bundle that is not intended for reuse lives in the src directory of the application and normally
has no vendor prefix.

Code organization
-----------------

Controllers
-----------

A controller is responsible for generating a response from a request. It is the main entrypoint
for executing business code and calls other services and often renders templates or other output.

A controller normally returns a Response object (or its subclasses such as JsonResponse, 
RedirectResponse, BinaryFileResponse, StreamedResponse). It can also return arbitrary values in
which case an event listener for the kernel.view event must generate the Response object.

Controller classes are by convention located in the Controllers directory of a bundle. There they can
be auto-discovered by the framework if they extend the base Controller class.

Controller classes derived from the base Controller class provide these shortcut methods:

- generateUrl() (generates a URL for a given route)
- forward() (forwards the request to another controller as an internal sub-request)
- redirect() (redirects to a URL)
- redirectToRoute() (redirects to a route)
- addFlash() (adds a flash message to the current session)
- isGranted() (checks if the current user has a certain permission)
- denyAccessUnlessGranted() (throws an exception if the current user does not have a certain permission)
- renderView() (renders a view and returns the rendered string)
- render() (renders a view and returns a response)
- stream() (streams a view)
- createNotFoundException() (creates an exception with HTTP status 404)
- createAccessDeniedException() (creates an exception with HTTP status 403)
- createForm() (creates a form instance)
- createFormBuilder() (creates a form builder instance)
- getDoctrine() (returns the Doctrine registry)
- getUser() (returns the current user)
- has()/get()/getParameter() (dependency injection container methods)
- isCsrfTokenValid() (returns if a given CSRF token is valid)

Alternatively controllers can be defined as services in the service container (independently of
if they are located in the Controllers directory or not). The advantage is that dependency 
injection can be used to inject exactly the required services instead of having to rely on the
Service Locator semi-anti-pattern the base controller uses. The disadvantage is that every 
dependency needs to be declared by the developer.

https://symfony.com/doc/current/controller.html
https://en.wikipedia.org/wiki/Service_locator_pattern

The views
---------

View are normally written in a templating language such as Twig. Twig is included in the Symfony
distribution, but plain PHP templates or other templating languages can be used as well.

Views are normally located in the Resources/views directory of a bundle or in the 
app/Resources/views directory of the application.

If the controller extends the base Controller, there is a render() method shortcut available. 

https://symfony.com/doc/current/templating.html

The resources
-------------

Resources are files that are not PHP code or templates, such as images, CSS or JavaScript files.

Normally they are located in the Resources/public directory of a bundle or in the 
app/Resources/public directory of the application.

Overriding default error pages
------------------------------

Error pages can be modified in three ways:

- Override the default error templates.
  - Place the custom template in app/Resources/TwigBundle/views/Exception.
  - Templates are named like "error<status>.<format>.twig", e.g. error404.html.twig.
  - Templates can therefore be overridden for each error and format independently.
  - If a template for a status code does not exist, use the more generic name for the format,
    e.g. error.json.twig.
  - If a template for the format does not exist as well, use the HTML template error.html.twig.
  - Exception pages can be tested by registering routes predefined in the TwigBundle.
- Override the default exception controller.
  - Create a new controller.
  - in the config.yml file, set twig: exception_controller to this controller.
- Register custom event listeners that listen to the kernel.exception event.
  - Set a Response object in the GetResponseForExceptionEvent that is passed to the listener.
  - Propagation of the event is stopped as soon as the first listener set a Response.

https://symfony.com/doc/current/controller/error_pages.html

Bundle inheritance
------------------

- A bundle can override parts of another bundle by extending the getParent() method in the 
  bundle class and specifying the bundle to inherit from.
- Then create resources in the Resources directory with the same path and name of the original
  resources to override them.
- controllers can be overridden by creating controllers with the same name. When a controller is
  called using the Bundle:Class:ControllerMethod syntax (only), the controller from the inheriting
  bundle will be called instead.
- Templates can only be overridden in the application, by placing them in the 
  app/Resources/<bundle_name>/views directory.
- Translations can only be overridden by loading translations with the same keys after the original
  ones, by registering overriding bundles after overridden bundles in the AppKernel (which is not
  allowed according to the bundle structure best practices). Alternatively, set translations in
  the app/Resources/translations directory, as messages in this directory will always override those
  in bundles.
- Validation rules cannot normally be overridden. It is necessary to create a new validation group
  for the changed rules and modify the bundle configuration to use the new group.

https://symfony.com/doc/current/bundles/inheritance.html

https://symfony.com/doc/current/templating/overriding.html

https://symfony.com/doc/current/bundles/override.html#override-translations

https://symfony.com/doc/current/bundles/override.html#override-validation

Event dispatcher and kernel events
----------------------------------

Semantic configuration and compiler passes
------------------------------------------

Semantic configuration:

With semantic configuration it is possible to configure a bundle. It is possible to apply
validation and default values to config values.
 
Semantic configuration consists of these parts:
- A configuration class implementing the ConfigurationInterface interface. This class returns
  a config tree builder that contains rules for validating the config values (ensuring that
  required values are configured and values have the correct type).
- An extension class that extends the `Extension` class. This class receives the config value
  and calls the configuration processing with the configuration class. Depending on the
  configuration, the extension can e.g. load different service definition files or set
  the config values as service arguments.
  The extension class should follow a naming scheme to be auto-discovered by the framework:
  - Be located in the DependencyInjection directory of the bundle.
  - Have the same name as the bundle class, but with an "Extension" suffix instead of "Bundle".

The config values can then be set in the config.yml (or similar) files of the application.
Each bundle has a config namespace that equals the bundle name minus the "Bundle" suffix and
converted to snake case. The Configuration class needs to define this namespace in the config
tree builder. Symfony will then pass all values in this namespace to the extension class.

As config values can be set on different levels, e.g. both in the config.yml and in the
config_dev.yml, there is a resolution algorithm that selects the correct values in the
processConfiguration() method of the extension. Extensions may choose another resolution
though, as processConfiguration() needs to be called manually and the extension receives
all values in an array of arrays.

A shortcut for implementing `load()` and calling `processConfiguration()` is to derive
the extension from `ConfiguratbleExtension` which provides the `loadInternal()` method.
This method receives the merged configuration.

A bundle can prepend configuration for another bundle so that it acts as default value
provider if config values are not set for the other bundle. To do this, the extension
needs so implement the PrependExtensionInterface and provide the config in the prepend()
method.

In contrast to a container parameter, a configuration parameter is only available in the
extension's load() method, not in the service container.

The best practices suggest to use semantic configuration only for distributed bundles,
not in a simple application.

Compiler passes:

A compiler pass is a modification of the service container. With semantic configuration it
is only possible to modify the current bundle (or to prepend settings), a compiler pass has 
access to the complete service container and can therefore modify arbitrary container items.
A compiler pass is registered in the bundle's build() method. A compiler pass name should
have the suffix "Pass" by convention.

A common use case for a compiler pass is to collect all services with a specific tag and
to provide these services to some other service.

https://symfony.com/doc/current/bundles/extension.html

https://symfony.com/doc/current/bundles/configuration.html

https://symfony.com/doc/current/bundles/prepend_extension.html

https://symfony.com/doc/current/best_practices/configuration.html#semantic-configuration-don-t-do-it

https://symfony.com/doc/current/service_container/compiler_passes.html

Controllers
===========

Naming conventions
------------------

- Controller classes should be located in a `Controller` directory.
- Controller classes should have a "Controller" suffix.
- Controller methods should have an "Action" suffix.

Using these conventions, a controller (which denotes the method, not the class) can be
auto-discovered during routing and be referenced by the short-hand syntax
Bundle:Class:Method (the Method part does not contain the "Action" suffix, it will be
appended automatically when called).

https://symfony.com/doc/current/bundles/best_practices.html#directory-structure

https://symfony.com/doc/current/controller.html#a-simple-controller

The base Controller class
-------------------------

See the `Controllers` chapter above.

The request
-----------

The request class can be automatically injected into the controller. To do this,
use a method argument of type `Request`.

https://symfony.com/doc/current/controller.html#the-request-object-as-a-controller-argument

The response
------------

A controller should return a `Response` object or a sub-class, such as the built-in sub-classes
`JsonResponse`, `RedirectResponse`, `BinaryFileResponse`. It can also return arbitrary values in
which case an event listener for the kernel.view event must generate the Response object.

https://symfony.com/doc/current/controller.html#the-request-and-response-object

https://symfony.com/doc/current/components/http_foundation.html#response

The cookies
-----------

Cookies can be set on the response, using $response->headers->setCookie().
By default a cookie is set to HTTP-only and can thus not be used in JavaScript on the client
side. This can be changed through the constructor of the cookie.

https://symfony.com/doc/current/components/http_foundation.html#setting-cookies

http://api.symfony.com/3.2/Symfony/Component/HttpFoundation/Cookie.html#method___construct

The session
-----------

The session can be accessed in two ways:

- from the request object, using $request->getSession()
- by injecting the `session` service (or accessing it using an injected container instance, 
  such as in the base controller class).

Note: There is ongoing discussion on the session being a service, as it is - like the request -
a data object. Maybe the session service will be removed in the future. From Symfony 3.3 on,
the session object can be injected into a controller the same way as the request.

https://symfony.com/doc/current/components/http_foundation/sessions.html

https://github.com/symfony/symfony/issues/10557

The flash messages
------------------

Flash messages are messages displayed to the user. These messages are displayed exactly once and
"survive" a redirect so that they can be displayed after a POST + redirect request, which is why 
they are saved in the session.

Flash messages can be added by calling $session->getFlashBag()->add(). The base controller has
a shortcut method addFlash(). Note that the `SessionInterface` does not provide the addFlash()
method, so it is currently required to rely on the implementation class `Session` instead.

Flash messages can be read from the flash bag using the get()/getAll() methods from the flash bag,
which will also delete the messages, or using peek()/peekAll() to return the messages without
deleting them. They can also be read from Twig templates (see example under the link below).

https://symfony.com/doc/current/controller.html#flash-messages

HTTP redirects
--------------

A request can be redirected by returning a `RedirectResponse` in the controller. It is possible
to set the HTTP status code in the response's constructor.

The base controller class also provides shortcut methods redirect() and redirectToRoute() which
also return a `RedirectResponse`.

https://symfony.com/doc/current/controller.html#redirecting

Internal redirects
------------------

A request can be redirected to another controller internally, which is called forwarding. 
For this purpose the base controller provides the forward() method that creates a sub-request 
based on the current request, setting the developer-provided controller.

As this uses a sub-request, forwarding is purely internal and there are no consequences for
the HTTP request.

https://symfony.com/doc/current/controller/forwarding.html

Generate 404 pages
------------------

There are three ways to return a 404 status:

- Return a response with this status code manually.
- Throw a `NotFoundHttpException`. Symfony will catch the exception and return the 404 status.
- When using the base controller, call createNotFoundException() which will create (but return,
  not throw) the exception.

https://symfony.com/doc/current/controller.html#managing-errors-and-404-pages

File upload
-----------

https://symfony.com/doc/current/controller/upload_file.html

Built-in internal controllers
-----------------------------

?
    
Routing
=======

https://symfony.com/doc/current/components/routing.html

Configuration (YAML, XML, PHP & annotations)
--------------------------------------------

https://symfony.com/doc/current/routing.html#routing-examples

Restrict URL parameters
-----------------------

Using the `requirements` key, URL parameters can be restricted by regular expressions.

Using the `schemes` key, a route can be restricted to a certain schme (HTTP, HTTPS).

https://symfony.com/doc/current/routing.html#adding-wildcard-requirements

Set default values to URL parameters
------------------------------------

- Annotations: Set default value for argument of the annotated controller method.
- YAML/XML: Set `defaults` key in the route definition.
- PHP: Set value in the defaults array argument.

https://symfony.com/doc/current/routing.html#giving-placeholders-a-default-value

Generate URL parameters
-----------------------

Trigger redirects
-----------------

Redirects can be triggered directly in the configuration file by specifying the
FrameworkBundle:Redirect:redirect and :urlRedirect controllers.

https://symfony.com/doc/current/routing/redirect_in_config.html

Special internal routing attributes
-----------------------------------

- _controller: Specifies the controller to use.
- _format: Specifies the request format (HTML, JSON, XML, ...).
- _fragment: Specifies the #
- _locale: Sets the request's locale (en, de, fr, ...).

- _route: The matched route name. Is set automatically by the router.
- _route_params: Parameters of the matched route. Is set automatically by the router.

https://symfony.com/doc/current/routing.html#special-routing-parameters

Domain name matching
--------------------

Using the `host` key, only allowing certain HTTP hosts is possible (placeholders may be used).

https://symfony.com/doc/current/routing/hostname_pattern.html

Conditional request matching
----------------------------

Using the `condition` key, an expression following the expression syntax can be defined.

https://symfony.com/doc/current/routing/conditions.html

HTTP methods matching
---------------------

Using the `methods` key, a route can be restricted to certain HTTP methods (GET, POST, ...).

https://symfony.com/doc/current/routing/requirements.html#adding-http-method-requirements

User's locale guessing
----------------------

- URLs should contain a locale part if a route is available in multiple languages, so that every
resource has a unique URL.
- The _locale placeholder should be used if the language is set dynamically (otherwise _locale
  can be set in the defaults key of the route) so that the translator gets automatically 
  configured.
- The request also provides a getPreferredLanguage() method that returns the first item of the
  intersection of the user-defined accepted locales (defined in the browser settings and 
  transmitted in the Accept-Language HTTP header) and the locales available in the application.
- The locale can then be made sticky by registering an event listener for kernel.request (see
  link below).


https://symfony.com/doc/current/translation/locale.html#the-locale-and-the-url

https://symfony.com/doc/current/session/locale_sticky_session.html

Router debugging
----------------

- The console command debug:router can be used to print all registered routes.
- The console command router:match can be used to test which route matches to a passed URL.

https://symfony.com/doc/current/routing/debug.html

Templating with Twig
====================

Auto escaping
-------------

- Output is automatically HTML-escaped in Twig to prevent XSS attacks. Auto-escaping can be
turned off for a string by applying the `raw` filter.
- It is also possible to specify `autoescape` blocks to auto-escape everything within that block.

https://symfony.com/doc/current/templating/escaping.html

https://twig.sensiolabs.org/doc/2.x/api.html#escaper-extension

Template inheritance
--------------------

- A template can extend another template by specifying "{% extends other-template.html.twig %}"
at the beginning (should be the first tag in the template, not sure if this is enforced).
- A sub-template can only change block contents, so the parent template needs to declare those
  blocks first.
- A sub-template's block definition overrides a parent's block definition completely, but the
  sub-template can make a {{ parent() }} call to render the parent block, similar to PHP's 
  method inheritance.
- Templates can also be extended dynamically, by passing a variable to the extends tag; any
  twig expression that returns a template name is allowed, so it is possible to use e.g. the
  ternary operator.
- It is also possible to pass an array to extends; then the first template that exists will be
  used.
- With the `use` tag, the blocks of another template are imported into the current template.
  This is only possible if the used template does not extend another template, does not define
  macros and if the body is empty (only blocks are allowed, as well as the `use` tag).
- The `use` tag does not allow expressions, the used template must be named literally.
- Blocks in the used template with the same name as blocks in the current template are ignored,
  unless they are renamed in the use statement ('use "template.html.twig" with someblock as
  otherblock').
- If two templates imported with `use` define a block with the same name, the latest template
  wins.

https://twig.sensiolabs.org/doc/2.x/templates.html#template-inheritance

https://twig.sensiolabs.org/doc/2.x/tags/extends.html

https://twig.sensiolabs.org/doc/2.x/tags/use.html

Global variables
----------------

- Built-in global variables are:
  - _self: current template name.
  - _context: current context.
  - _charset: current charset.
- Symfony adds the `app` global variable which itself contains variables for:
  - debug
  - environment
  - request
  - security
  - session
  - user
- Custom global variables can be configured by setting `twig: globals: foo: bar` in the config.yml file.
- The value can also be a service; set the service ID as value, prefixed with an "@".

https://symfony.com/doc/current/templating/global_variables.html

https://symfony.com/doc/current/reference/twig_reference.html#app

Filters and functions
---------------------

- Filter are modifiers for variables or hard-coded values. They are applied by appending a pipe
  "|" to the value to modify, followed by the filter name.
- A filter may have further arguments. These are passed in brackets after the filter, resembling
  a function call.
- Functions are in form and syntax similar to PHP functions. A major difference is that when
  calling a method with arguments, these arguments can optionally be named instead of relying
  on the position. Example: foo(bar='baz).
- Named arguments can also be applied to filters.
- Besides built-in filters and functions, it is possible to define custom ones.
  - Filters and standalone Twig: Instantiate a Twig_Filter object and call addFilter() on the
    Twig_Environment.
  - Filters and Symfony: Create a class that extends Twig_Extension. Let the getFilters() method
    return an array of Twig_Filter objects. Register the extension in the dependency injection
    container by creating a service for the extension and adding the tag twig.extension.
  - Functions and standalone Twig: Instantiate a Twig_Function object and call addFunction() on the
    Twig_Environment.
  - Functions and Symfony: Like creating filters, but use getFunctions() instead of getFilters().
    An extension can provide both filters and functions.

https://twig.sensiolabs.org/doc/2.x/templates.html#filters

https://twig.sensiolabs.org/doc/2.x/templates.html#functions

https://twig.sensiolabs.org/doc/2.x/templates.html#named-arguments

https://twig.sensiolabs.org/doc/1.x/filters/index.html

https://twig.sensiolabs.org/doc/1.x/functions/index.html

https://twig.sensiolabs.org/doc/2.x/advanced.html#filters

https://symfony.com/doc/current/templating/twig_extension.html

Template includes
-----------------

- The `include` tag includes and renders another template.
- The included template can be given as string literal or expression.
- It is possible to pass an array of template names. The first of these templates that exists
  will be included.
- By default, the included template has access to the complete context of the current template.
- Additional variables can be passed as array using the `with` keyword.
- To deny access to the current context, use the `only` keyword.
- `with` and `only` can be combined.

https://twig.sensiolabs.org/doc/1.x/tags/include.html

Loops and conditions
--------------------

Loops:
- Only the `for` tag is available for loops, no while.
- Start a loop with `for` ... `in` ... `if` (if is optional), add an optional `else` and end the loop with `endfor`
- The `if` keyword's right-hand side is executed within loop context and can therefore access
  the current loop element. The special `loop` variable should not be used in the condition.
- The `else` branch is executed if there are no elements to iterate over.
- It is possible to iterate over any sequence, which is either:
  - an array
  - an object of a class implementing `Traversable`
  - an explicit sequence using the `..` operator, e.g. 0..10 or 'a'..'z'
  - an expression evaluating to one of the other possibilities.
- In every loop, the special `loop` variable, which provides various loop properties,
  is available (see the link below for details).
- The properties `loop.length`, `loop.revindex`, `loop.revindex0` and `loop.last` rely on the
  iteratee implementing `Countable` or being an array. Also they do not work for loops with
  `if` conditions.
- Loops are scoped in twig, so a variable declared with `set` will not be available outside the loop
  (TODO this should mean that the current loop element is also unavailable after the end of the loop).
- Loop execution can NOT be modified by break or continue. Use `if` to skip some elements that
  do not match a condition.
- Iterating over keys is possible by using the `keys` filter on the array. Keys can be accessed
  by using the `for key, value in array` syntax.
  
https://twig.sensiolabs.org/doc/1.x/tags/for.html

https://twig.sensiolabs.org/doc/1.x/tags/set.html

Conditions:

- `if` is similar to PHP's `if`.
- The same wacky rules as for PHP's `==` operator apply to determine if a value is true-ish.
- Strict comparisons (the equivalent of `===`) can be performed with the test `is same as`.
- To check if a variable is defined, `if var is defined` can be used (important to distinguish
  unset variables from those evaluating to false, e.g. if the variable contains 0).
- Further possibilities in `if` tags are:
  - comparisons (<, <, <=, ..., `starts with`, `ends with`, `matches` )
  - containment (`in`, `not in`)
  - test (see links below)

https://twig.sensiolabs.org/doc/1.x/tags/if.html

https://twig.sensiolabs.org/doc/1.x/tests/sameas.html

https://twig.sensiolabs.org/doc/1.x/templates.html#comparisons

https://twig.sensiolabs.org/doc/1.x/templates.html#containment-operator

https://twig.sensiolabs.org/doc/1.x/templates.html#test-operator

https://twig.sensiolabs.org/doc/1.x/tests/index.html

URLs generation
---------------

- The Symfony twig integration provides two functions for URL generation: 
  - `path` generates a relative URL for a route
  - `url` generates an absolute URL for a route
- Custom twig extensions are defined in the twig bridge.
- Links to assets are described below.
- When generating URLs from the console, it is necessary to configure the request context
  in order to let the console know of the base URL; either globally by parameters, or
  by getting the context of the router and setting values there, e.g. to specify different
  values for different controllers.

https://symfony.com/doc/current/templating.html#linking-to-pages

https://symfony.com/doc/current/console/request_context.html

Controller rendering
--------------------

- To include the results of a controller in a template, use the render(controller('foo'))
  syntax.
- The controller must be given in the string syntax bundle:controller:action.

https://symfony.com/doc/current/templating/embedding_controllers.html

Translations and pluralization
------------------------------

- Strings can be translated by using `trans` and `transchoice`, both of which are available
  both as tag and as filter.
- Difference between tag and filter: When using the filter, output escaping is applied 
  automatically, but NOT when using the tag.
- `trans` translates a simple strings using optional placeholders.
- `transchoice` adds support for pluralization; different messages can be defined for different
  amounts (discrete values or ranges).
- A message domain can be passed by using the `from` keyword.
- The target language can be passed by using the `into` keyword followed by the ISO 639-1 code
  of the language.
- A default translation domain for a template can be defined by adding the 
  {% trans_default_domain "mydomain" %} tag to the template (will NOT be inherited by included
  templates). 

https://symfony.com/doc/current/translation.html#translations-in-templates

String interpolation
--------------------

- Arbitrary expressions can be inserted into a double-quoted (!) string by using the 
 `#{expression}` syntax. The result of the expression is inserted into the string.

https://twig.sensiolabs.org/doc/2.x/templates.html#string-interpolation

Assets management
-----------------

- Links to assets like images, CSS, JavaScript are generated using the `asset` function.
- This generates a relative URL. Absolute URLs can be generated with `absolute_url(asset('foo.png))`
- Best practice: Use template inheritance to add styles/scripts that are generally used 
  throughout the application in a parent template, add styles/scripts that are used only 
  in single templates in those templates. Use separate blocks for styles and for scripts
  so that they can be extended separately. 
- Symfony: Assets can be copied or symlinked to the web directory (or somewhere else) by 
  using the console command `assets:install`. Assets will then by default be copied/symlinked
  to web/bundles/bundlename; the assets need to be located in Resources/public in the bundle.
- the bundlename in the previous bullet point is all-lower-case without a "bundle" prefix and
  without underscores.
- composer update and install will also copy/symlink assets if the script 
  `ScriptHandler::installAssets` is set as post-update-cmd/post-install-cmd in composer.json.
- There are framework configuration options to set a `base_path` (relative to the web root),
  `base_urls` (absolute URLs, e.g. for a CDN), `version` (suffix for generated asset URLs,
  to "clear" browser caches), `version_format` (sprintf format string that allows to define
  a custom format for version string appending). Beginning with Symfony 3.1 there is also
  a parameter `version_strategy`.
- The framework configuration options can also be defined for multiple `packages`. These are
  independent "buckets" of assets for which the aforementioned parameters can be set.


https://symfony.com/doc/current/templating.html#linking-to-assets

https://symfony.com/doc/current/templating.html#including-stylesheets-and-javascripts-in-twig

https://symfony.com/doc/current/reference/configuration/framework.html#assets

Debugging variables
-------------------

- Variables can be output for debugging purposes by using the `dump` function.
- This function is not enabled by default, but needs to be activated by adding the extension
  Twig_Extension_Debug.
- When using the DebugBundle and the Symfony full-stack framework, the `dump` function will
  be available automatically. 
  - When calling {% dump(foo) %}, the content of foo will be dumped into the debug toolbar
    in order to avoid markup modification.
  - {{ dump(foo) }} will dump the content directly into the markup.
- when NOT using xdebug, the `<pre>` HTML tag is recommended for better formatting.

https://twig.sensiolabs.org/doc/2.x/functions/dump.html

https://symfony.com/doc/3.0/components/var_dumper.html#debugbundle-and-twig-integration

Forms
=====

Forms creation
--------------

- Create a POPO class with getters and setters (or public properties if you don't mind ugly
  code). This is the data model.
- Create a form builder:
  - With base controller: Call createFormBuilder($objectOfDataModelClass).
  - With Dependency Injection: Inject form.factory and call createBuilder() on the instance.
- Call add() on the form builder to add fields.
- Call getForm() to build the actual form.

This declares the form inline. It is recommended that a form type class is created instead so
that the type can be reused and form logic is generally separated from controller code (see
below).

https://symfony.com/doc/current/forms.html#creating-a-simple-form

https://symfony.com/doc/current/forms.html#building-the-form

Forms handling
--------------

- Forms are by default submitted to the same URL they were rendered, but using the POST method.
- So controllers normally handle both form creation and submission.
- Call handleRequest() to process the form. On GET requests nothing will happen; on POST
  requests, submitted data will be transferred to the data model object given at form creation;
  additionally the form will be validated if submitted.
- Check if data was submitted by using $form->isSubmitted() and $form->isValid(). If yes,
  continue with form handling logic; if no, continue with form display logic.
  
Form validation actually validates the data model object linked to the form, not the form 
itself. Define validation on the data model as described in the validation chapter below.

https://symfony.com/doc/current/forms.html#handling-form-submissions

Form types
----------

- Create a class that extends AbstractType and override buildForm(). This method receives
  an object implementing FormBuilderInterface on which the form can be built as directly
  in the controller.
- To apply this class, use one of these two ways:
  - With base controller: Call createForm() and pass the class name of the new form type.
  - With Dependency Injection: Inject form.factory and call create() on the instance,
    also passing the class name of the new form type.

https://symfony.com/doc/current/forms.html#creating-form-classes

Forms rendering with Twig
-------------------------

In the controller, pass the result of $form->createView() to the template. This object can
then be used to render the form in the template. There are a few helper functions. 

Simple mode:

- form_start() begins the form.
- form_widget() renders the form elements.
- form_end() finishes the form (renders the </form> tag as well as rendering all remaining
  form elements that were not rendered manually yet, including the CSRF token (see the
  CSRF protection section below)).

Advanced mode:

- form_errors() renders error messages encountered during form processing.
- form_row(form.element) renders the label, input element error messages and the input element.

Instead of using form_row() it is also possible to render all elements manually with these
functions:

- form_label(form.element)
- form_errors(form.element)
- form_widget(form.element)

To access individual values of an element, use this syntax:

form.element.vars.value

https://symfony.com/doc/current/forms.html#rendering-the-form

https://symfony.com/doc/current/form/rendering.html

https://symfony.com/doc/current/reference/forms/twig_reference.html

Forms theming
-------------

- Forms are rendered using Twig templates.
- In these templates there are blocks that render the fragments of the form.
- Override the templates and modify the blocks to change the form theme.
- To apply this theme, 
  - either add the tag `form_theme` to the top of the template rendering the form.
  - or set "twig: form_themes: " in the config.yml in order to set the form globally.
- Blocks not overridden will be taken from the default theme.
- Read the link below for much more details.

https://symfony.com/doc/current/form/form_themes.html

CSRF protection
---------------

- With CSRF an attacker exploits that a user is logged into a system and can therefore 
  unknowingly execute privileged actions that are beneficial for the attacker and/or 
  non-beneficial for the victim. For this purpose the attacker introduces a link to the
  HTML markup that the victim's client then calls, e.g. in an image source attribute.
- The Form component automatically protects from CSRF attacks by adding a CSRF token in
  all forms.
- The CSRF token is saved in the session and rendered as hidden field `_token` into the 
  form when `form_end()` is called.
- The token prevents attackers from rebuilding the form and tricking the user into 
  submitting, because the CSRF token is unknown outside the application.
- The token can be disabled by setting the form option `csrf_protection` to false. This
  can be done in the `configureOptions()` method of the form type.
- Further CSRF options are `csrf_field_name` (name of the token in the rendered form) and
  `csrf_token_id` (which allows to create a different CSRF token for every form type).
- When using a cache, care must be taken in that the token is not cached across users
  as token validation will else fail for all users but the first.
  - Use ESI fragments the exempt the forms from caching.
  - Load the form via uncached Ajax.
  - Load just the token via Ajax and set/replace the value in the form.
- To protect against CSRF without using the session, solutions like Double Submit Cookie,
  Encrypted Token Pattern or Custom Request Headers can be used (see the link to OWASP
  below for details and drawbacks).

https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)

https://symfony.com/doc/current/form/csrf_protection.html

https://symfony.com/doc/current/http_cache/form_csrf_caching.html

https://www.owasp.org/index.php/Cross-Site_Request_Forgery_%28CSRF%29_Prevention_Cheat_Sheet

Handling file upload
--------------------

- Add a field of FileType type to the form type (and a corresponding property to the data
  model). Render the field as any other type.
- Handle the form in the controller and the property of the data model will be populated
  with an object of type `UploadedFile`.
- This file should be moved to a custom directory by using the `move()` method on the file.
- Security best practice: Determine the file extension by calling `guessExtension()` method
  on the file, which will guess based on the MIME type instead of the user-provided extension
  which could be spoofed.
- Security best practive: Give the file a custom/random file name instead of the original one
  which could be crafted by an attacker (or apply proper sanitation).

https://symfony.com/doc/3.0/controller/upload_file.html

Built-in form types
-------------------

https://symfony.com/doc/3.0/reference/forms/types.html

Data transformers
-----------------

- There are two types of data transformers: Model transformers and view transformers.
  I haven't yet understood when to use which transformer, maybe you have better luck
  looking at the docs :-)
- Data transformers translate values from an internal object representation to an
  HTML-renderable string and back, e.g. translates between a DateTime object and a
  string in yyyy-MM-dd format.
- A data transformer is attached to a form field in the `buildForm()` method by calling
  `$builder->get('elementName')->addModelTransformer($transformer)`, where $builder is
  the method's `FormBuilderInterface` object and $transformer is an object of a class
  implementing `DataTransformerInterface`.
- The `CallbackTransformer` is an implementation of `DataTransformerInterface` that
  takes custom callback functions for `transform()` and `reverseTransform()`.

https://symfony.com/doc/current/form/data_transformers.html

Form events
-----------

- Form events can be used to react to certain steps during the processing of a submitted
  form.
- The form component uses the Symfony event dispatcher component to handle form events
  (event dispatcher is generally required by form).
- The lifecycle of a submitted form looks like this:
  - New form.
  - FormEvents::PRE_SET_DATA is dispatched.
  - `setData()` is executed.
  - FormEvents::POST_SET_DATA is dispatched.
  - FormEvents::PRE_SUBMIT is dispatched.
  - `submit()` is executed and FormEvents::SUBMIT is dispatched.
  - FormEvents::POST_SUBMIT is dispatched.
- Details on this process should be looked up in the documentation (see link below).
- Form events are attached using the method `addEventListener()` on the form builder.
- Alternatively an event subscriber can be attached by calling `addEventSubscriber()` on
  the form builder.
- Form events only affect the current instance of a form type, while code in the form type
  itself affects all instances; this could be important if there are multiple forms or
  fields of the same type on a single request.

https://symfony.com/doc/3.0/form/events.html

https://symfony.com/doc/3.0/form/dynamic_form_modification.html

Form type extensions
--------------------

- Form type extensions extend an existing form type. 
- They influence both the selected form type and all types that extend this form type
  (e.g. the built-in TextType is the parent class of EmailType, SearchType and others,
  which will also be affected by an extension to TextType). Form type extensions for
  FormType and ButtonType will affect almost all types.
- A form type extension is a class that extends `AbstractTypeExtension` or implements
  `FormTypeExtensionInterface`. The class must implement the `getExtendedType()` method
  that returns the fully qualified class name of the form type to extend.
- The following methods can be overridden/implemented. They are called at the end of
  their counterparts in the form type (e.g. `buildForm()` of the extension will be called
  at the end of the form type's `buildForm()` method), so the extension has access to
  the results of the basic methods.
  - buildForm()
  - buildView()
  - finishView()
  - configureOptions()
- To enable the extension, register it as a service and apply the tag `form.type_extension`.
- When changing how the form field is displayed, it will be necessary to override the
  respective template fragments (see templating chapter).

https://symfony.com/doc/3.0/form/create_form_type_extension.html

https://symfony.com/doc/current/form/data_transformers.html#creating-a-reusable-issue-selector-field

https://symfony.com/doc/current/form/create_custom_field_type.html
    
Data Validation
===============

PHP object validation
---------------------

- Any object can be validated. Normally data model classes are validated.
- Validation can be applied to classes, class properties or methods (getters).
  Not all constraints support all of these types, though. Constraint classes can override
  the `getTargets()` method of the base `Constraint` class to define valid targets.
- Properties either need to be public or have public getters that follow the naming
  convention of beginning with get, is, has followed by the property name (CamelCase).
- Validation rules can be defined either using annotations in the validated class,
  a `validation.yml` or `validation.xml` file located in the `Resources/config`directory
  of a bundle or by implementing a public static method named `loadValidatorMetadata()`.
- When defining validation rules in separate files, a rules for a method is defined using
  a hypothetical property name that method is a getter of (so no "get" prefix).
- It is also possible to validate a raw value programmatically without any configuration.
  Get the `validator` service and call the `validate()` method, passing the value to
  validate and the constraint. This will return a `ConstraintViolationList` object.

https://symfony.com/doc/3.0/validation.html

https://symfony.com/doc/3.0/validation/raw_values.html

Built-in validation constraints
-------------------------------

https://symfony.com/doc/3.0/validation.html#supported-constraints

Validation scopes
-----------------

Looks like this means constraint targets which are already described above.

Validation groups
-----------------

- For every validation rule, an optional list of validation groups can be defined.
- When executing the validation, a list of validation groups can be passed. Only
  validation rules in these groups will be applied (a single matching group suffices).
- Validation rules without an explicit group declaration are always part of the `Default`
  group. Rules are also part of this group if they are part of a group named like the
  class name or a group explicitly named `Default`.
- There is always a group named like the class (NOT fully qualified?). This group is
  similar to the `Default` group, but differs if the class contains embedded objects.
  See the docs for an explanation (first link below).
- Which validation groups to use can be determined dynamically by creating a class with
  an `__invoke()` method that accepts a `FormInterface` as only parameter and passing an
  instance of this class to the `OptionsResolver` in the `FormType`'s `configureOptions()`
  method (method `setDefaults()`, key `validation_groups`).

https://symfony.com/doc/3.0/validation/groups.html

https://symfony.com/doc/3.0/validation/group_service_resolver.html

Group sequence
--------------

- Validation groups can be applied sequentially to avoid overwhelming the user with
  error messages. Define a group sequence, listing the validation groups in the order
  in which they should be applied (see the link below for the exact syntax).
  Only if all constraints in a group are valid, the next group is validated.
- In a group sequence the `Default` group refers to the group sequence, not to all
  constraints without a group. This may lead to infinite recursion if the `Default`
  group is defined as part of the group sequence itself.
- A group sequence can also be defined dynamically. The validated class needs to implement
  `GroupSequenceProviderInterface` and its `getGroupSequence()` method which returns a list
  of validation groups. The class then also needs to be marked as supporting the group
  sequence provider (see link below).

https://symfony.com/doc/3.0/validation/sequence_provider.html

Custom callback validators
--------------------------

- Use the built-in `Callback` constraint on a public method that expects a
  `ExecutionContextInterface` parameter.
- Call `buildViolation()` on the execution context to add a validation error determined
  through the custom logic.
- It is also possible to define this method as static. Then it will be passed the object
  to validate as first argument. Don't know why anyone should want to use this, but it's
  there :-)
- A PHP callable can be configured so that validation can take place outside the class
  to validate. This must be a static method the interface of which is the same as in the
  case before. It is not possible to pass a service method or a global function.
  When configured via PHP, it is possible to pass a closure.

https://symfony.com/doc/3.0/reference/constraints/Callback.html

Violations builder
------------------

TODO
    
Dependency Injection
====================

Service container
-----------------

- A service is a simple PHP class which is additionally referenced in service configuration 
  in XML, YAML or PHP format.
- A service definition consists basically of an `id` and a declaration of the `class` 
  implementing that service.
- The service container holds all services defined in the application.
- Services can be retrieved from the container directly by using the `get()` method on the
  container instance (service locator pattern, often regarded as anti-pattern) or by 
  injecting them into other services.
- Injecting is done in the service definition. Either add an `argument` to the service that 
  should get the dependency injected, so that this service will be passed as constructor 
  argument. Or declare a method `call` on a setter. Using constructor injection makes sure
  that the service can't be instantiated without all dependencies given and that these
  dependencies aren't changed during the request. Using setter injection allows for optional
  dependencies.
- It is also possible to inject directly into public properties with `property` declarations,
  but .. no.
- If a service that is injected does not exist, a `ServiceNotFoundException` will be thrown.
  This behavior can be adjusted by setting the `on-invalid` parameter on the argument to
  either `null` (not when using YAML for configs) or `ignore`. These values are identical
  when used for `argument`s in that a null value is injected. When used for `call`s, `null`
  will inject a null value, while `ignore` will remove the method call when the service ID is
  invalid.
- Controllers as entry points for business code either get services injected or extend the
  base controller class and do then automatically have a container instance available.
- When using the full-stack framework, the service container is automatically dumped into PHP
  code, so that the heavy compile process does not need to take place on every request. The
  compiled container is a class loaded at the beginning of the request.
- Service instantiation is lazy. A service is only constructed when it is used.
- When a service is instantiated, all services passed as constructor arguments are 
  instantiated as well (and their arguments and so on, creating the whole dependency graph).
- To avoid building the complete dependency graph (with a possible performance impact which
  might not be worthwhile if the heavy service isn't always used), services can be marked 
  as `lazy`. Then the instantiation of services passed as constructor arguments will only 
  take place when those injected services are used for the first time. 
  Proxy objects will be injected then, which will "resolve" to the actual objects when used. 
  This requires installing an additional library ocramius/proxy-manager (if not using the
  full-stack framework, symfony-proxy-manager-bridge is also required). It is generally 
  recommended to avoid doing a lot of work in constructors.
- Services are normally only instantiated once. Further uses of the service receive the same
  instance so that instantiation and initialization work only need to be done once per 
  request. Therefore services should be regarded as immutable after instantiation (do not 
  modify internal state during regular usage).
- To receive a new service instance on each call, define it with the parameter 
  `shared` = false. This is "infective": Non-shared services can only be injected into other
  non-shared services.
- By default services are public and can be retrieved by the container's `get()` method.
  To avoid this, mark them as private by setting the property `public` = false. If the
  service is only injected once, the service will be inlined into the container and the
  definition removed. If it is injected more than once, the service will behave like a
  public service and can be retrieved with `get()`. In later Symfony versions private 
  services will be truly private.
- Aliases allow to refer to services under other names (e.g. for shortcuts or maintain
  backward compatibility). Use the `alias` property to reference the original service.
- Services can be marked as deprecated using the `deprecated` property. The property value
  needs to be a message that is displayed as warning when that service is used. The parameter
  %service_id% must be used in that message.
- For a list of registered services, execute the console command `debug:container`.
- Services can be decorated by other services by using the `decorates` property which 
  needs to reference the original service. The decorator service will then completely replace
  the original service and is registered under the same name. The original service can be
  injected into the decorator by using a special service ID that is the same as the 
  decorator + `.inner`.
  If multiple decorators are defined for a service, the order will be dependent on the order
  in which the bundles are registered. The order can be set explicitly by adding a 
  `decoration_priority` property.
  The service's implementation should also implement the decorator pattern.
- Parent services can be defined to declare common dependencies for multiple services.
  Use the `parent` property. The service with this property will then inherit all settings
  but shared, abstract and tags. The implementations may have an inheritance relation but
  this is not required as long as e.g. called methods with the same name exist. Dependencies
  of the parent service can be overridden by adding another argument and specifying the
  index of the argument to override.
- A service can be declared as `abstract` to avoid instantiating. This can make sense for
  base services only intended to serve as parent for other services. No `class` needs to
  be provided for abstract services.
- Synthetic services are services that are constructed during runtime instead of container
  compile time. For these services, a stub service is defined in the service configuration,
  using the `synthetic` = true property, so that other service definitions depending on the 
  synthetic service will not cause errors during container compilation. The synthetic service
  is injected into the container by calling the `set()` method on the container instance.
  When the service (or another service depending on this one) is used before it was 
  constructed, a `RuntimeException` is thrown.

https://en.wikipedia.org/wiki/Dependency_injection

https://symfony.com/doc/3.0/service_container.html

https://symfony.com/doc/3.0/service_container/injection_types.html

https://symfony.com/doc/3.0/service_container/optional_dependencies.html

https://symfony.com/doc/3.0/service_container/alias_private.html

https://symfony.com/doc/3.0/service_container/lazy_services.html

https://symfony.com/doc/3.0/service_container/service_decoration.html

https://symfony.com/doc/3.0/service_container/parent_services.html

https://symfony.com/doc/3.0/service_container/synthetic_services.html

Built-in services
-----------------

Use the console command debug:container on a new Symfony project to get a list of all
built-in services (when providing the option `--show-private`, private services will
be displayed, except those that are only injected into a single service, because the
definition will be inlined in the compiled container and the definition removed).

https://symfony.com/doc/3.0/service_container/debug.html

Configuration parameters
------------------------

- Besides services, parameters can also be defined for the service container. Parameters
  are simple key-value associations that can be injected into services. Use the `parameters`
  section in a config file to define parameters. They can be accessed via %parameter_name% (
  so the % is reserved at the beginning of a string in the configuration. Use %% to escape
  the percent character).
- Parameters can also be retrieved from the service container using the `getParameter()`
  method.

https://symfony.com/doc/3.0/service_container/parameters.html

Services registration
---------------------

See above.

Tags
----

- A tag is a special marker for a service declaration that indicates that the service has
  a special meaning during configuration.
- A compiler pass (see below) can get a list of all services tagged with a specific tag
  (by calling `findTaggedServiceIds()` on the provided service container instance) and e.g. 
  inject the service collection into another services. This way, decoupled and distributed 
  definition of a service list is possible.
- Tags always have a `name` and can optionally have additional properties. Apart from the
  name, no properties have a special meaning and can be used by the compiler pass in any
  way. The name itself is only a string that identifies the tag. There is no registry of
  tags; by defining a name, the tag is defined.

https://symfony.com/doc/3.0/service_container/tags.html

Semantic configuration
----------------------

- By default there is an /app/Resources/services.yml file in which services for the 
  application are defined. This file is imported in the config.yml file which itself is 
  imported in conf_<env>.yml which is loaded in the AppKernel.
- How to define services in a bundle is already described in the "Bundles" chapter of this
  document.

https://symfony.com/doc/3.0/bundles/extension.html

https://symfony.com/doc/3.0/bundles/configuration.html

Factories
---------

- To programmatically create a service instance, a call to factory can be defined instead
  of relying solely on static container configuration.
- A factory can be either a static method of a class or a method of another service defined
  in the service container.
- Syntax (XML): Tag `factory`, attributes `class` or `service` and `method`.
- If a factory is defined for a service, the configured `class` value is ignored - only the
  factory return value is relevant. It is nevertheless recommended to set a sensible value
  as compiler passes may rely on this information.
- `argument`s will be passed to the factory instead of being applied to the service 
  definition itself.

https://symfony.com/doc/3.0/service_container/factories.html

Compiler passes
---------------

Compiler passes are already described in the "Bundles" chapter of this document.

Services autowiring
-------------------

- With autowiring the service container can be instructed to automatically resolve
  dependencies for a service so that they do not need to be specified manually, thus
  saving time for rapid prototyping.
- In the constructor of a class to be defined as autowired service, declare the types of
  dependencies in order to allow the service container compiler to discover which services
  to inject.
- In the service definition, set the property `autowired` = true.
- If there is only one service definition for the dependency, the container will automatically
  determine that this service is to be injected.
- This also works when an interface is type-hinted instead of a concrete class.
- If there is no service definition for the dependency, the container automatically creates
  a private service for the class. This does not work for interfaces.
- If there are multiple implementations of an interface, it is required that a default
  implementation is specified. Define a service for the implementation and provide the
  `autowiring_types` property, setting the interface for which this implementation is the
  default. If this isn't done, a RuntimeException will be thrown because of the ambiguous
  resolution. Note that `autowiring_types` is deprecated as of Symfony 3.3.

- Autowiring should not be used in public bundles or complex projects.

https://symfony.com/doc/2.8/service_container/autowiring.html

https://symfony.com/blog/new-in-symfony-3-3-deprecated-the-autowiring-types

Security
========

Authentication
--------------

- Authentication is the process of identifying who the user is.
- The user storage is represented by one or more user providers.
- User passwords should always be hashed. This is done by encoders.

https://symfony.com/doc/3.0/security.html

Authorization
-------------

- Authorization is the process of determining what a user (who is known after authentication)
  is allowed to do.
- Authorization rules are given by access control rules based on the request URI so that sections
  of the site can be protected.
- Authorization can also be checked programmatically by calling the `isGranted()` method on the 
  implementation of `AuthorizationCheckerInterface` received by injecting `security.authorization_checker`.
  The base controller class has a shortcut `isGranted()` for this method.
- There is also an `is_granted()` Twig function to check access in templates.

https://symfony.com/doc/3.0/security.html

Configuration
-------------

- Security is provided by the `SecurityBundle`, to the bundle alias and therefore the
  configuration key is `security`. Normally security is configured in a separate config file
  named `security.yml`/`security.xml`.
- There's a lot that can be configured in the security context. Some of these configuration options
  are described below, but have a look at the docs in any case.

https://symfony.com/doc/3.0/security.html

Providers
---------

- A user provider is the source of user login data.
- Built-in user providers are:
  - `memory`: Users are directly configured in the configuration file.
  - `entity`: Users are loaded from a database via Doctrine.
  - `ldap`: Users are loaded via LDAP.
  - `chain`: Allows to combine multiple user providers, e.g. to define some users
    in the configuration and fetch others from the database.
- Providers are configured under the `providers` key. Every provider to be used should be added as
  named array key with its configuration under that key.
- Custom providers can also be implemented (see link below). Short story: Create a class that implements
  `UserProviderInterface` and define a service for this class. In the firewall configuration (see below),
  add a provider with a custom name under  `providers` and for this provider set the `id` key to the
  new service.

https://symfony.com/doc/3.0/security.html#b-configuring-how-users-are-loaded

https://symfony.com/doc/3.0/security/entity_provider.html

https://symfony.com/doc/3.0/security/multiple_user_providers.html

https://symfony.com/doc/3.0/security/custom_provider.html

Firewalls
---------

- A firewall defines how users authenticate. This can be any type of HTTP-based 
  authentication like basic authentication, password, OAuth, ...
- For every authentication type there is a config key:
  - `anonymous` if anonymous access is allowed.
  - `x509`
  - `remote_user`
  - `http_basic`
  - `http_basic_ldap`
  - `http_digest`
  - `guard`
  - `form_login`
  - `form_login_ldap`
- It is also possible to create a custom authentication type (which is according to the
  docs quite hard and is normally better avoided if possible).
- Multiple firewalls can be configured. Restrictions can be defined for a firewall. The 
  security system will check all firewalls from top to bottom and the first applicable
  firewall is used for this request.
- Possible restrictions are:
  - pattern: A regex which the request URL needs to match.
  - host: A regex which the host/domain part of the request URL needs to match.
  - methods: An array of HTTP methods the request method needs to match (most common are
    GET and POST).
- TODO what happens if no firewall matches?
- A firewall can be deactivated with the key `security` = false. This should be combined
  with restrictions, e.g. to grant access to debug toolbar resources or assets to all users. 
- A firewall can be configured to use a specific user provider by setting the `provider`
  key. Unless this is done, the first user provider in the list of providers will be used. 

https://symfony.com/doc/3.0/security.html#a-configuring-how-your-users-will-authenticate

https://symfony.com/doc/3.0/security/firewall_restriction.html

https://symfony.com/doc/3.0/reference/configuration/security.html

Users
-----

- Users are represented by a user class that implements either `UserInterface` or 
  `AdvancedUserInterface`.
- `UserInterface` contains these methods:
  - `getRoles()` - returns all roles granted to the user.
  - `getPassword()` - returns the hashed password.
  - `getSalt()` - returns the salt used to hash the password. May be null if the used hashing
    algorithm handles salt internally, such as bcrypt.
  - `getUsername()` - returns the username used as login name.
  - `eraseCredentials()` - removes sensitive data from the object. Implementations should delete
    critical data such as the plaintext password from the user object in this method.
- `AdvancedUserInterface` extends `UserInterface` and adds some possibilities to block login for
  different reasons. This allows for fine-grained ways of controlling authentication, including
  different messages displayed to the user on authentication failuer. Methods specified by this 
  interface are:
  - `isAccountNonExpired()` - if this method returns false, the authentication system will throw
    an `AccountExpiredException` on a login attempt.
  - `isAccountNonLocked()` - if this method returns false, the authentication system will throw
    a `LockedException` on a login attempt.
  - `isCredentialsNonExpired()` - if this method returns false, the authentication system will throw
    a `CredentialsExpiredException` on a login attempt.
  - `isEnabled()` - if this method returns false, the authentication system will throw
    a `DisabledException` on a login attempt.
- The user object must be serializable/unserializable so that it can be saved in the session.
- On login, the user object is serialized into the session. On every request, the ID is taken from that
  user to get a fresh user object from the database.
- It is not possible to access users during routing as routing is handled before security.
  Also it is not possible to restrict 404 pages.

https://symfony.com/doc/3.0/security.html

Passwords encoders
------------------

- Encoders are misnamed password hashing classes.
- An encoder is defined in the `encoders` configuration key. Allowed values depend partially
  on the used PHP version, but supported values are normally `plaintext`, `sha512`, `pbkdf2`,
  `bcrypt`. The currently (May 2017) recommended algorithm is `bcrypt`.
- To manually hash a password, e.g. for saving the user in a database, inject the service
  `security.password_encoder` and call `encodePassword()`, passing the user object and the
  plain password. The password encoder will select the correct password hashing algorithm
  based on the user class.
- The plain password must not be longer than 4096 characters in order to avoid possible
  denial-of-service attacks (the hashing cost increases significantly with the plaintext
  length for most hashing algorithms).
- A password for in-memory users can be hashed using the console command 
  `security:encode-password`.

https://symfony.com/doc/3.0/security/password_encoding.html

https://symfony.com/blog/cve-2013-5750-security-issue-in-fosuserbundle-login-form

Roles
-----

- Roles are simple strings that set groups of permissions for a user.
- A role name must always begin with the prefix `ROLE_`.
- A user's role is acquired by the security system by calling the `getRoles()` method on the user
  object (implementing `UserInterface`) after login. The roles need to be persisted along with the other
  user data.
- To limit the number of roles that need to be assigned to a user, inheritance rules for roles can be 
  defined under the `role_hierarchy` key of the `security` configuration. For example, a ROLE_SUPER_ADMIN
  can contain the ROLE_ADMIN, so that a super admin only needs the first role and gains the second one
  automatically.
- There are three role-like entities that can be used to check if a user is logged in:
  - IS_AUTHENTICATED_REMEMBERED is set on a user if she is logged in, either directly or with a remember-me
    functionality.
  - IS_AUTHENTICATED_FULLY is set on a user if she is logged in directly, but not if she is only remembered.
    This can be used to force re-authentication on critical operations.
  - IS_AUTHENTICATED_ANONYMOUSLY is set on all users, regardless of login state.
  
https://symfony.com/doc/3.0/security.html#roles

https://symfony.com/doc/3.0/security.html#checking-to-see-if-a-user-is-logged-in-is-authenticated-fully

Access Control Rules
--------------------

- Access control rules are a list of rules that define which roles a user need to have to access a certain
  section of the site. For example, a "^/admin" section could be restricted to users with ROLE_ADMIN.
- Matching options are:
  - `path`: Regex to match the path of the current request URL against.
  - `ip`/`ips`: IP address of the client needs to match given IP addresses.
  - `host`: Regex to match the domain part of the current request URL against.
  - `methods`: A list of HTTP methods, e.g. GET and POST.
- Symfony will check all rules from top to bottom and apply the first one whose pattern matches the current
  request URL.
- After a rule matched, it is checked if the user has access to the section. For this, the following 
  attributes can be configured:
  - `roles`: A list of roles the user needs to have to gain access. If this condition is not fulfilled,
    an `AccessDeniedException` will be thrown which normally redirects the request to a login form (if
    the user is already logged in, an access-denied page will be displayed).
  - `allow_if`: An expression that allows access only if it evaluates to true.
  - `requires_channel`: If set to e.g. `https`, the request will only be allowed if the request is an
    HTTPS request. Otherwise it will be redirected to the corresponding HTTPS URL. Alternatively this can 
    be specified in the routing configuration.
- To block access for all users, set the `roles` attribute to a non-existing role such as ROLE_NO_ACCESS.
  This can be used e.g. to restrict access to certain client IP addresses: Add a rule for with `ip` matching
  option and a second one with ROLE_NO_ACCESS. The first rule will allow access to all users from the
  configured IP address while it will not match for other users. The second rule will then reject all 
  requests from other IP addresses.

https://symfony.com/doc/3.0/security/access_control.html

https://symfony.com/doc/3.0/expressions.html#security-complex-access-controls-with-expressions

https://symfony.com/doc/3.0/security/force_https.html

Guard authenticators
--------------------

- Guard authenticators easen the implementation of a custom authentication system.
- A guard authenticator is a class that either implements `GuardAuthenticatorInterface` or extends 
  `AbstractGuardAuthenticator`. The abstract class implements one of the interface methods, 
  `createAuthenticationToken()`.
- The interface defines seven methods that are described in the docs and in the code. These methods 
  basically handle user retrieval, credential provision and post-authentication hooks.
- Register the authenticator in the firewall configuration under 
  `security: firewalls: myfirewall: guard: authenticators: myauthenticatorserviceid`. Besides the
  authenticator(s), an `entry_point` (see below) and a (user) `provider` can be specified. 
- Multiple authenticators are supported. The list of authenticators will be iterated, and any authenticator
  that returns `null` for `getCredentials()` will be skipped (exceptions will cancel the authentication
  process).
- When using multiple authenticators, the `entry_point` must be configured. This is the service ID of
  the authenticator on which the `start()` method should be called if the user provided no credentials.
- To use different entry points for different authenticators, multiple firewalls need to be configured.

https://symfony.com/doc/3.0/security/guard_authentication.html

https://symfony.com/doc/3.0/security/multiple_guard_authenticators.html

Voters and voting strategies
----------------------------

- Voters are a way to programmatically check if access to a certain object should be granted.
- A voter is an implementation of `VoterInterface`. It is registered as a voter by tagging its service
  definition with `security.voter`.
- Voters are decoupled from the client code checking permissions. The client code only asks for access to
  some "attribute" of an object. This attribute is a custom operation on the object, such as "view" or 
  "edit". Which voter(s) check access is not visible to the client code.
- When client code asks is access is granted to the current user (by calling `isGranted()` on the injected
  `security.authorization_checker`), the voter system (more precise, the `security.access_decision_manager`
  service) iterates over all registered voters and calls their `vote()` methods. `vote()` returns one of
  `VoterInterface::ACCESS_GRANTED`, `VoterInterface::ACCESS_ABSTAIN`, `VoterInterface::ACCESS_DENIED`.
- The total result of the votes depends on the strategy used for the `AccessDecisionManager` (which is
  configured in the `security: access_decision_manager: strategy` config key). Possible values are:
  - `affirmative` (default): Access is granted if at least one voter grants access.
  - `consensus`: Access is granted if more voters grant access than deny access.
  - `unanimous`: Access is granted if all voters grant access.
- Voters can also extend the `Voter` class instead of implementing the `VoterInterface` interface. In this
  case, they need to implement the methods `supports()` (which should return true if the voter feels
  responsible to handle the passed combination of subject and operations/attributes) and `voteOnAttributes()`
  (which should return a boolean value - true to grant access, false otherwise). There is no way to abstain
  from the vote - `VoterInterface::ACCESS_ABSTAIN` will only be return by the `Voter` base class if the
  class is not responsible.
- Normally only one voter exists for a check (i.e. there is exactly one voter that returns true in 
  `supports()` for a checking request; other voters return false in `supports()`, or ACCESS_ABSTAIN if they
  do not extend `Voter` but implement `VoterInterface`).
- The base controller has shortcut methods `isGranted()` (directly calls the method with the same name
  in `security.authorization_checker`) and `denyAccessUnlessGranted()` (calls `isGranted()` and throws an
  `AccessDeniedException` if access is denied).
- If no voter supports the authorization request, or all voters return ACCESS_ABSTAIN, the outcome depends 
  on settings for the `AccessDecisionManager`. See the code for that class for details.
- Notable built-in voters are:
  - `AuthenticatedVoter`: Performs authentication checks mentioned above (IS_AUTHENTICATED_FULLY, ...).
  - `RoleVoter`: Performs authorization (role) checks mentioned above.
  - `RoleHierarchyVoter`: Subclass of `RoleVoter` that resolves role the hierarchy defined in security.yml.
  - `ExpressionVoter`: Votes on attributes that are `Expression` objects.

https://symfony.com/doc/3.0/components/security/authorization.html

https://symfony.com/doc/3.0/security/voters.html

HTTP Caching
============

Cache types (browser, proxies and reverse-proxies)
--------------------------------------------------

- The browser can be instructed to cache a response, so that the browser won't repeat the same request
  as long as the cache is valid (time-based expiration cache using the headers `Expires` and `Cache-Control`,
  or validation cache using the headers `ETag` and `Last-Modified`, or a combination of both expiration
  and validation).
- A reverse proxy sits between the client and the application. It interprets the cache directives in the 
  application's response and caches these responses for all clients. It is also possible to cache only
  fragments of a page.
- When using a shared cache such as a gateway cache, care needs to be taken if user-specific data is
  contained in the page, be it personal information such as address data or technical information such
  as a CSRF token.

https://symfony.com/doc/3.0/http_cache.html

https://symfony.com/doc/3.0/http_cache/varnish.html

https://symfony.com/doc/3.0/http_cache/form_csrf_caching.html

Expiration (Expires, Cache-Control)
-----------------------------------

- Expiration is more simple and efficient than validation and should therefore be preferred when possible.
- Either use the header `Cache-Control` to set a cache duration in seconds (Symfony: use 
  `Response::setSharedMaxAge()`).
- Or use the header `Expires` to set a date and time value (in GMT timezone) for when the cache should 
  expire (Symfony: use `Response::setExpire()`). The date should not be more than one year in the future
  according to the HTTP/1.1 spec.

https://symfony.com/doc/3.0/http_cache/expiration.html

Validation (ETag, Last-Modified)
--------------------------------

- With validation caching, the browser can be instructed to check for cache validity. This can be used to
  update a resource as soon as it changed on the server side.
- The server responds with HTTP status 304 if the cache is still fresh, or the regular response with status
  200 if not.
- This type of cache is mostly only useful if the system needs less time to check for cache validity than 
  to generate the full response.
- Either use the header `ETag`. The ETag is any custom value the server can use to check if the cache is
  still fresh. The server sets the ETag on the response and the client re-sets the same ETag on the next
  request for the same URL (Symfony: use `Response::setETag()` in combination with `Response::setPublic()`
  and `Response::isNotModified()`).
- Or use the header `Last-Modified`. The server sets this value to a date at which the resource was last 
  modified (this may depend on multiple internal modification dates, e.g. if the requested resource contains
  blog post and author data). The client sets the request header `If-Modified-Since`, and if this date is
  later than the `Last-Modified` date, the server will return the 304 status code and will not continue
  processing the request. This check is performed in the `Response::isNotModified()` method.

https://symfony.com/doc/3.0/http_cache/validation.html

Client side caching
-------------------

Server side caching
-------------------

Edge Side Includes
------------------

- Normally gateway caches cache the entire page. If a page contains dynamic elements that may not be cached,
  Edge Side Includes can be used to integrate these dynamic elements in the otherwise statically cached page.
- It is also possible to set a differing cache lifetime for these includes. 
- There is a tag `<esi:include />` that marks a spot as an insertion point for a page 
  fragment. The `src` attribute contains the absolute path to the resource to include. The gateway cache
  will automatically fetch this sub-resource from the application and insert it into the page markup before
  returning the response to the client (or read the fragment from its cache storage if the fragment is
  cached as well).
- In Symfony there is the `render_esi()` Twig function that can be used in the same way as the standard
  `render()` function (and therefore be used in conjunction with `controller()` and `url()`). If a gateway
  cache is used, the include tag will be generated; else the content is generated as if the `render()`
  function was used.
- Symfony detects if a gateway cache is installed by checking if a request header `Surrogate-Capability`
  is present and contains `ESI/1.0`. If the Symfony reverse proxy is used, this header is not required.
- Configuration: Set `framework: esi: { enabled: true }`.
- As sub-resources need to be directly fetchable via HTTP, Symfony needs care of fragment routing. Configure
  `framework: fragments: { path: /_fragment }` to let routing add a /_fragment prefix to any URLs generated
  through the `render_esi()` function and resolve the route on requests.
- Fragment routes are only resolved if they are signed, so that clients cannot call these routes directly.
- Use only the `s-maxage` response header instead of `maxage` when working with ESI to prevent the client
  from applying the cache expiration settings of the complete page instead of the fragments.

https://symfony.com/doc/3.0/http_cache/esi.html

Console
=======

Built-in commands
-----------------

- The list of built-in commands can be viewed by calling `bin/console` without parameters.
- The probably most often used built-in command is `cache:clear`.
- By default, the `dev` environment is used. Specify e.g. the prod environment with `--env=prod` or set the 
  environment variable SYMFONY_ENV to the desired value.

Custom commands
---------------

- Custom commands are classes that extend the `Command` or the `ContainerAwareCommand` class.
- A command is auto-detected by Symfony if it lives in the `MyBundle/Command` namespace of a bundle and
  if its name ends with `Command`.
- Commands can also be registered as services, so that e.g. dependencies can be declared. To be recognized
  as command, a service needs to be tagged with `console.command`.
- A command needs to implement a `configure()` method, in which its name, description, help text, input
  options and arguments are defined.
- A command needs to implement an `execute()` method, which contains the actual command code.
- Optionally commands may also implement an `initialize()` method which is called after input arguments
  have been set. This can be used e.g. for command class hierarchies to set base class variables in the
  sub-classes.
- Optionally commands may also implement an `interact()` method which is called after `initialize()`, but
  before `execute()`. In this method the command may interact with the user in order to receive missing
  arguments.

https://symfony.com/doc/3.0/console.html

https://symfony.com/doc/3.0/console/commands_as_services.html

Configuration
-------------

- A command can be configured by overriding the `configure()` method. This method is called by the constructor
  of the `Command` class (therefore be sure to call the parent if you override the constructor of a command).
- In the `configure()` method, call the following methods to configure the command:
  - `setName()`: Sets the name for the command by which it can be called from the terminal. The name should be namespaced,
    using colons as separators, e.g. `myapp:mybundle:mycommand`. Alternatively the name can directly be passed in the
    constructor.
  - `setDescription()`: Sets the short description that is displayed when the user executes the `list` command (which is also
    the default if no command is passed).
  - `setHelp()`: Sets the longer help text that is displayed when the user executes the command with the `--help` option.
  - `setCode()`: Sets a callable to be run instead of the code in the `execute()` method.
  - `setDefinition()`: Defines a set of arguments and options for the command (see below).
  - `setProcessTitle()`: Sets the title of the system process for this command (intended for long-running commands only).
  - `setAlias()`: Sets one or more aliases for the command.

https://symfony.com/doc/3.0/console.html#configuring-the-command

Options and arguments
---------------------

- Arguments are strings passed with the console call. They are always ordered.
- To define an argument, call `$this->addArgument()` in the command's `configure()` method, providing a name and 
  optionally further values. To get the value, call `InputInterface::getArgument('argumentname')` in the `execute()`
  method.
- Arguments can be required or optional. This can be set with the second parameter for `addArgument()` (value
  `InputArgument::REQUIRED` or `InputArgument::OPTIONAL`).
- An argument can also be an array. Set `InputArgument::IS_ARRAY` as type. Only the last argument can be an array.
  The array can be combined with REQUIRED/OPTIONAL by combining them with logical OR, e.g. 
  `InputArgument::REQUIRED | InputArgument::IS_ARRAY`
- If a required argument was not passed by the user, the console will output an error and abort.
- An argument can have a description which will be displayed when the command is called with `--help`. This is the 
  third parameter for `addArgument()`.
- With the fourth and last parameter, a default value can be set, but only if the argument is optional.
- Options are unordered modifiers for the command. They are always passed with a double dash, e.g.  `--force`; an 
  abbreviation can be defined, so that the option can be passed with a single dash and normally a single letter, e.g. 
  `-f` (second parameter for `addOption()`).
- To define an option, call `$this->addOption()` in the command's `configure()` method, providing a name and 
  optionally further values. To get the value, call `InputInterface::getOption('optionname)` in the `execute()` method.
- Options are always optional.
- Options are by default boolean flags, but can be configured to accept values. To do this, set the third parameter of
  `addOption()` to `InputOption::VALUE_REQUIRED` or `InputOption::VALUE_OPTIONAL`. The value `InputOption::VALUE_NONE` 
  can also be set explicitly. The value `InputOption::VALUE_ARRAY` marks the option as array, which can be combined with
  optional or required as for arguments.
- Description and default (parameters 4 and 5) behave as for arguments. 

https://symfony.com/doc/3.0/console/input.html

Input and Output objects
------------------------

- In the command's `execute()` method, an `InputInterface` and an `OutputInterface` are available.
- Input is described in the sections above (options, arguments) and below (questions).
- The output object provides methods `write()` and `writeln()` to write content to the current output.
- Verbosity levels are also handled by the output object, this is discussed below.
- Output can be styled by surrounding the emphasized text with tags, such as `<info>`, `<comment>`, `question`, `error`.
- Custom styles can be defined by instantiating an `OutputFormatterStyle`, setting foreground and background colors as
  well as options like bold text. This style can be set by calling `$output->getFormatter()->setStyle('name', $style)`.
- To simplify output formatting, the Symfony Style Guide can be applied to format output by its semantics, e.g. 
  setting text as title will automatically apply title formatting. This is done by instantiating `SymfonyStyle` and 
  calling its methods like `title()`, `block()`, `section()`, `listing()`, `text()`, `comment()`, `success()`, 
  `error()`, `warning()`, `note()`, `caution()`, `table()`, `ask()`, `askHidden()`, `confirm()`, `choice()`, 
  `progressStart()`, `progressAdvance()`, `progressFinish()`, `createProgressBar()`, `askQuestion()`, `newLine()`. Some
  of these methods use the helpers described below.

https://symfony.com/doc/3.0/console/coloring.html

https://symfony.com/doc/3.0/console/style.html

Built-in helpers
----------------

- Most helpers can be retrieved with `Command::getHelper()`, passing the name of the helper.
- Question helper (`question``): Helps in asking the user for input. Has one method `ask()` that returns user input depending on the
  question class provided.
  - Ask simple yes/no questions with a `ConfirmationQuestion`; returns a boolan value.
  - Ask free text questions with a `Question`; returns a string.
  - Ask for the selection from a list with a `ChoiceQuestion`; returns a string. Can also be configured to allow 
    multiple answers separated by a comma; will then return an array containing strings.
  - Autocomplete values can be provided that will automatically be suggested as the user types.
  - Input can be hidden, e.g. for passwords.
  - A validation callback function can be set so that user input is automatically validated (throw an exception in the
    function when validation fails).
  - A unit test can use an input stream to emulate user input (`fopen('php://memory', 'r+', false)`).
- Formatter helper (`formatter`): Helps in formatting output with color.
  - Print output in a section with `formatSection()` (colored output and with the section name in square brackets at the
    beginning of the line).
  - Print output in a block with `formatBlock()` (colored background with the background not depending on the length of
    the individual lines; predefined styles like `error` (red background) can be used, or custom ones defined).
- Progress bar (instantiate `ProgressBar`): Helps in displaying progress information on long-running tasks.
  - Instantiate `ProgressBar` and call `start()` on the instance.
  - Do the work the progress bar indicates, e.g. in a loop and advance the progress bar after each step by calling 
    `advance()`. The progress bar can also be configured to advance only on fewer calls of `advance()` by calling
    `setRedrawFrequency()`. Additionally, the progress can be advanced by more than one step by calling `advance(int)` 
    or be set manually with `setProgress()`.
  - Call `finish()` at the end to display the 100% state.
  - There are lots of customization options for the progress bar; see the docs.
- Table (instantiate `Table`): Helps in displaying tabular data (the old table helper was removed in Symfony 3.0).
  - Instantiate `Table`.
  - Set headers with `setHeaders()`.
  - Set data with `setRows()`.
  - Optionally add separator rows with instances of `TableSeparator` in the table row data.
  - Optionally add table cells that span multiple columns with a `TableCell` instance in the table row data with the
    `colspan` setting set to the desired value (similar to the HTML table colspan attribute).
  - Optionally add table cells that span multiple rows. This is also done with a `TableCell` instance, but with a
    `rowspan` setting.
  - Optionally set the table style with `setStyle()`; values are `default`, `compact` (no separators between rows or 
    columns), `borderless` (no separators between columns). Custom styles can be defined (see docs).
  - Call `render()`.
- Debug formatter helper (`debug_formatter`): Helps in displaying debug information for different processes run by the
  command. 
  - The helper formats messages for starting a process, output and error information while running this process
  and a result after stopping the process.
  - The methods `start`, `progress` and `stop` expect an identifier as first argument, so that multiple processes run by
    the command can be distinguished. Passing `spl_object_hash($process)` is recommended.
- Process helper (`process`): Helps in running external processes.
  - Call `run()` or `mustRun`, passing the command to run. Both will run the command and return the executed process,
    but `mustRun()` will throw a `ProcessFailedException` if the command was not successful (i.e. the exit code was not 
    0).
  - The process helper uses the debug formatter helper.

https://symfony.com/doc/3.0/components/console/helpers/questionhelper.html

https://symfony.com/doc/3.0/components/console/helpers/formatterhelper.html

https://symfony.com/doc/3.0/components/console/helpers/progressbar.html

https://symfony.com/doc/3.0/components/console/helpers/table.html

https://symfony.com/doc/3.0/components/console/helpers/debug_formatter.html

Console events
--------------

- Similar to web requests, console commands dispatch some events during their lifecycle which can be used as hooks for
  custom actions.
- `ConsoleEvents::COMMAND` / `console.command`: Dispatched before a command is run. Use it e.g. to log command execution
  or disable the command dynamically by calling `ConsoleCommandEvent::disableCommand()`. In the latter case, the console
  will return exit status 113.
- `ConsoleEvents::EXCEPTION` / `console.exception`: Dispatched when an exception is thrown. Use it to handle exceptions,
  e.g. log the exception.
- `ConsoleEvents::TERMINATE` / `console.terminate`: Dispatched after the command was executed. Use it e.g. to perform
  cleanup operations. Listeners may also change the exit code.

https://symfony.com/doc/3.0/components/console/events.html

Verbosity levels
----------------

- The console supports 5 verbosity levels that can be used to control the amount of output messages.
- Commands need to explicitly/programmatically output messages conditionally based on the current 
  verbosity level, or otherwise the level requested by the user has no effect. The check can be done by
  either calling `OutputInterface::getVerbosity()` and comparing the value to the constants 
  `OutputInterface::VERBOSITY_*`, or calling `OutputInterface::isVerbose()` 
  and so on. Note that quiet mode is automatically considered by Symfony, so this level does not need to be checked 
  manually.
- If an exception occurs, the full stack-trace will be printed on all levels of verbose and above.
- Defined levels are:
  - quiet: no output; constant `VERBOSITY_QUIET`, method `isQuiet()`, argument `-q` or `--quiet`.
  - normal: default level; constant `VERBOSITY_NORMAL`, no method, no argument.
  - verbose: increased verbosity; constant `VERBOSITY_VERBOSE`, method `isVerbose()`, argument `-v`.
  - very verbose: informative, non-essential messages; constant `VERBOSITY_VERY_VERBOSE`, method `isVeryVerbose()`, argument `-vv`.
  - debug: debug messages; constant `VERBOSITY_DEBUG`, method `isDebug()`, argument `-vvv`.

https://symfony.com/doc/3.0/console/verbosity.html
    
Automated Tests
===============

- To run tests with PHPUnit, call `phpunit` from the project root. This assumes that PHPUnit was installed globally.
- PHPUnit will use a `phpunit.xml` file in the same directory as configuration file, or `phpunit.xml.dist` if no
  `phpunit.xml` file was found.
- A standard Symfony project comes with a `phpunit.xml.dist` file, which among other settings sets the bootstrap path
  to `app/autoload.php`, so that Composer autoloading is automatically available.

https://symfony.com/doc/3.0/testing.html

Unit tests with PHPUnit
-----------------------

- Just as regular PHPUnit unit testing, nothing special here.

https://symfony.com/doc/3.0/testing.html#unit-tests

Functional tests with PHPUnit
-----------------------------

- For functional tests, make an initial request, check the response, click a link or submit a form, check the response,
  and so on.
- There is a `WebTestCase` which a functional test should extend in order to have the Symfony container booted and a
  client object with lots of helper methods available (`createClient()` method). The `WebTestCase` extends the 
  `KernelTestCase` which extends the default `PHPUnit_Framework_TestCase`.


https://symfony.com/doc/3.0/testing.html#functional-tests

Client object
-------------

- The `Client` object acts like a browser and can therefore be used to send requests and examine the responses.
- A call to `Client::request()` will return a `Crawler` object for XML and HTTP responses that allows programmatic
  access to the response, so that it is much easier to make assertions on the contents (Symfony DOMCrawler component).
- For other content types, the raw response can be received with `Client::getResponse()->getContent()`.

Crawler object
--------------

https://symfony.com/doc/3.0/testing.html#testing-crawler

Profile object
--------------

- The profile can be retrieved by calling `getProfile()` on the client. The profile can be null if profiling is disabled.
- Collectors can then be retrieved by calling `getCollector()` on the profile.

https://symfony.com/doc/3.0/testing/profiling.html

Framework objects access
------------------------

- The service container can be retrieved by calling `getContainer()` on the client. This should only rarely be used;
  instead it is better to examine the response to decide if a test was successful.

https://symfony.com/doc/3.0/testing.html#accessing-the-container

Client configuration
--------------------

- The client can be insulated in separate processes to avoid interference. This is done by simply calling `insulate()`
  on the client; internally the process component will be used to start another PHP process. This will be slower
  because of the overhead of creating new processes. It can make sense to use the main process for a request to avoid
  one additional process.
- By default, the client does not automatically follow redirects. The client can be instructed to follow a redirect by
  calling `followRedirect()`. It can also be configured to automatically follow all redirects by calling 
  `followRedirects()`.

https://symfony.com/doc/3.0/testing.html#working-with-the-test-client

https://symfony.com/doc/3.0/testing.html#redirecting

Request and response objects introspection
------------------------------------------

https://symfony.com/doc/3.0/components/dom_crawler.html

PHPUnit bridge
--------------

- The PHPUnit bridge adds a few Symfony-specific features to unit tests.
- Use a consistent locale across tests.
- Load Doctrine annotations if used.
- The `DeprecationErrorHandler` adds information on deprecated features in tested code in the summary at the end of the
  tests.
  - Deprecation notices that were silenced with "@".
  - Deprecation notices that were not silenced with "@". By default, any non-silenced tests will fail. This can be 
    configured by setting the environment variable SYMFONY_DEPRECATIONS_HELPER (e.g. in phpunit.xml in the <php> section 
    with `<env name="SYMFONY_DEPRECATIONS_HELPER" value="x" />`).
    - Set it to a number to configure a number of tests required to fail in order to let the test run fail.
    - Set it to "weak" to ignore deprecation errors.
    - Set it to a regex enclosed by "/" to output all deprecation messages matched by this regex.
  - Tests that test legacy features. These tests are marked in one of these ways:
    - Add the `@group legacy` annotation to the test class or method.
    - Let the test class name begin with `Legacy`.
    - Let the test method name begin with `testLegacy` instead of `test`.
    - Let the data provider method start with `provideLegacy` or `getLegacy` (bit weird to me).
- `ClockMock` helper class for time-sensitive tests. This class mocks the PHP time-based functions `microtime()`, 
  `sleep()`, `time()`, `usleep()`, so that calls to `sleep()` and `usleep()` will increase an internal timer instead of 
   actually sleeping, and the other functions will return the increased time to simulate the real `\sleep()` call.
- The `ClockMock` class will register the new functions in the namespace of the tested class, so that they override the
  default PHP functions. There are two ways to trigger this registration:
  - Add the `@group time-sensitive` annotation to the test class or method. Then the namespace is guessed from the 
    namespace of the test class by removing the `\Tests` part and assuming that apart from that the namespaces are the
    same. If this is not the case, the namespace needs to be configured in the phpunit.xml file (see docs).
  - Call `ClockMock::register(__CLASS__)` and `ClockMock::withClockMock(true)` before the test and 
    `ClockMock::withClockMock(false)` after the test.

https://symfony.com/doc/3.0/components/phpunit_bridge.html

Handling legacy deprecated code
-------------------------------

See above.

Miscellaneous
=============

Error handling
--------------

Code debugging
--------------

Deployment best practices
-------------------------

https://symfony.com/doc/3.0/deployment.html

Process and Serializer components
---------------------------------

https://symfony.com/doc/3.0/components/process.html

https://symfony.com/doc/3.0/components/serializer.html

https://symfony.com/doc/3.0/serializer.html

Data collectors
---------------

Web Profiler and Web Debug Toolbar
----------------------------------

Internationalization and localization
-------------------------------------








_____________________________________


Other Links
===========

https://leanpub.com/symfony-selfstudy

https://github.com/jmolivas/symfony-certification-guide

https://en.wikibooks.org/wiki/Symfony_3_Certification_Guide

https://trello.com/c/aHnP3WUI/1-learn-for-symfony-certification