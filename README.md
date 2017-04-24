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

Symfony Architecture
====================

Symfony Standard Edition
------------------------

License
-------

Components
----------

Bundles
-------

Bridges
-------

Configuration
-------------

Code organization
-----------------

Request handling
----------------

Exception handling
------------------

Event dispatcher and kernel events
----------------------------------

Official best practices
-----------------------

Release management
------------------

Backward compatibility promise
------------------------------

Deprecations best practices
---------------------------

Standardization
===============

Release management and roadmap schedule
---------------------------------------

Framework interoperability and PSRs
-----------------------------------

Naming conventions
------------------

Coding standards
----------------

Third-party libraries integration
---------------------------------

Composer packages handling
--------------------------

Development best practices
--------------------------

Framework overloading
---------------------

Semantic versioning
-------------------
    
Bundles
=======

Naming conventions
------------------

Code organization
-----------------

Controllers
-----------

The views
---------

The resources
-------------

Overriding default error pages
------------------------------

Bundle inheritance
------------------

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

