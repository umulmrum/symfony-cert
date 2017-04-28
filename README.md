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

Object Oriented Programming
---------------------------

Namespaces
----------

Interfaces
----------

Anonymous functions and closures
--------------------------------

- anonymous function: Function without a name, used inline as callback.
- closure: Function assigned to a variable, can be executed by accessing the variable and adding brackets + arguments.

https://en.wikipedia.org/wiki/Closure_(computer_programming)

https://secure.php.net/manual/en/functions.anonymous.php

Abstract classes
----------------

- class modifier "abstract".
- abstract methods (semicolon instead of method body) can be defined.
- cannot be instantiated directly, needs to be subclassed.
- concrete subclasses must define abstract methods.
- used as basic implementation, often to define a common workflow (e.g. template method pattern).

Exception and error handling
----------------------------

- traditionally errors are raised as notices, warnings, errors and fatal errors.
- those PHP errors are output to the browser and logged in the web server logs, both of which can be 
  disabled or limited to certain error types ("error_reporting" and "display_errors" - the latter 
  can only be completely enabled or disabled; both can be set in php.ini and using ini_set()).
- error output can be suppressed by adding an @ before a function or method call (though this is
  generally discouraged).
- errors cannot normally be treated in another way than logging and displaying; however, it is
  possible to register a custom error handler function using set_error_handler('functionName').
- a custom error handler receives information on the error type, the message, the file, the line
  number and a context.
- setting error_reporting so that e.g. a notice will not be reported means that a custom error handler
  is not triggered when a notice would be raised.
  
- exceptions are more common in object-oriented programming languages.
- exceptions are themselves classes that derive from \Exception.
- exceptions can be thrown by built-in and user code.
- when an exception is thrown, the current method execution is aborted and the exception object
  bubbles up the execution path until they are either caught in a method on a higher level, or 
  the outermost method call is reached. In the latter case, the thread is aborted and a stack trace
  is rendered (output can be disabled in PHP with display_errors).

https://secure.php.net/manual/en/errorfunc.configuration.php#ini.error-reporting

https://secure.php.net/manual/en/errorfunc.configuration.php#ini.display-errors

https://secure.php.net/manual/en/function.set-error-handler.php

Traits
------

- traits are additions to classes that can define methods and properties.
- a class can "use" a trait by adding the "use" keyword. The properties and methods of the trait
  will then behave as if copy-and-pasted into the class definition. This is a workaround for the
  fact that in PHP no multi-inheritance is allowed.
- if a class defines a method with the same name as a method in a trait, the class method overrides
  the one in the trait. However, a trait method overrides a method with the same name of a super
  class.
- a class can use multiple traits. If method names conflict, one of the methods must be selected
  using the "insteadof" keyword (e.g. B::foo instead of A if A and B are classes and foo is a method
  defined in both of them).
- an alias can be given to a method by using the "as" keyword (e.g. B::foo as bar). The method can
  then be called by its alias. In a naming conflict this makes it possible to use both methods by
  providing an alias for one of them.
- see link below for more features such as changing method visibility, traits composed from traits,
  abstract and static members.
- as traits can contain properties and not only methods, PHP traits are really mixins.
  
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

Controllers are by convention located in the Controllers directory of a bundle. There they can
be auto-discovered by the framework if they extend the base Controller class.

Controllers derived from the base Controller provide these shortcut methods:

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
    
Controllers
===========

Naming conventions
------------------

The base Controller class
-------------------------

The request
-----------

The response
------------

The cookies
-----------

The session
-----------

The flash messages
------------------

HTTP redirects
--------------

Internal redirects
------------------

Generate 404 pages
------------------

File upload
-----------

Built-in internal controllers
-----------------------------
    
Routing
=======

Configuration (YAML, XML, PHP & annotations)
--------------------------------------------

Restrict URL parameters
-----------------------

Set default values to URL parameters
------------------------------------

Generate URL parameters
-----------------------

Trigger redirects
-----------------

Special internal routing attributes
-----------------------------------

Domain name matching
--------------------

Conditional request matching
----------------------------

HTTP methods matching
---------------------

User's locale guessing
----------------------

Router debugging
----------------
    
Templating with Twig
====================

Auto escaping
-------------

Template inheritance
--------------------

Global variables
----------------

Filters and functions
---------------------

Template includes
-----------------

Loops and conditions
--------------------

URLs generation
---------------

Controller rendering
--------------------

Translations and pluralization
------------------------------

String interpolation
--------------------

Assets management
-----------------

Debugging variables
-------------------

Forms
=====

Forms creation
--------------

Forms handling
--------------

Form types
----------

Forms rendering with Twig
-------------------------

Forms theming
-------------

CSRF protection
---------------

Handling file upload
--------------------

Built-in form types
-------------------

Data transformers
-----------------

Form events
-----------

Form type extensions
--------------------
    
Data Validation
===============

PHP object validation
---------------------

Built-in validation constraints
-------------------------------

Validation scopes
-----------------

Validation groups
-----------------

Group sequence
--------------

Custom callback validators
--------------------------

Violations builder
------------------
    
Dependency Injection
====================

Service container
-----------------

Built-in services
-----------------

Configuration parameters
------------------------

Services registration
---------------------

Tags
----

Semantic configuration
----------------------

Factories
---------

Compiler passes
---------------

Services autowiring
-------------------
    
Security
========

Authentication
--------------

Authorization
-------------

Configuration
-------------

Providers
---------

Firewalls
---------

Users
-----

Passwords encoders
------------------

Roles
-----

Access Control Rules
--------------------

Guard authenticators
--------------------

Voters and voting strategies
----------------------------
    
HTTP Caching
============

Cache types (browser, proxies and reverse-proxies)
--------------------------------------------------

Expiration (Expires, Cache-Control)
-----------------------------------

Validation (ETag, Last-Modified)
--------------------------------

Client side caching
-------------------

Server side caching
-------------------

Edge Side Includes
------------------
    
Console
=======

Built-in commands
-----------------

Custom commands
---------------

Configuration
-------------

Options and arguments
---------------------

Input and Output objects
------------------------

Built-in helpers
----------------

Console events
--------------

Verbosity levels
----------------
    
Automated Tests
===============

Unit tests with PHPUnit
-----------------------

Functional tests with PHPUnit
-----------------------------

Client object
-------------

Crawler object
--------------

Profile object
--------------

Framework objects access
------------------------

Client configuration
--------------------

Request and response objects introspection
------------------------------------------

PHPUnit bridge
--------------

Handling legacy deprecated code
-------------------------------
    
Miscellaneous
=============

Error handling
--------------

Code debugging
--------------

Deployment best practices
-------------------------

Process and Serializer components
---------------------------------

Data collectors
---------------

Web Profiler and Web Debug Toolbar
----------------------------------

Internationalization and localization
-------------------------------------

