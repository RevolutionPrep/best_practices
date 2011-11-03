# Mission

  We value the leverage that can be gained through the usage of common practices in all of our programming processes. Having practices that define a 'best' approach for solving a particular problem allows us to evaluate and improve our practices on a continual basis. What follows is a living document of the best practices for building applications at Revolution Prep. We acknowledge that this document is incomplete and may suggest practices that lose favor over time. We consider this document to be a best effort at defining the collective discussion of our programming practices.

## Table of Contents

* [Ruby](#ruby)
    * [Formatting](#formatting)
    * [Syntax](#syntax)
    * [Know Your Language](#language)
    * [Naming](#naming)
    * [Comments](#comments)
    * [Annotations](#annotations)
    * [Classes](#classes)
    * [Exceptions](#exceptions)
    * [Collections](#collections)
    * [Strings](#strings)
    * [Percent Literals](#literals)
    * [Miscellaneous](#misc)
    * [Design](#design)
* [Rails](#rails)
    * [Configuration](#configuration)
    * [Routing](#routing)
    * [Controllers](#controllers)
    * [Models](#models)
    * [Migrations](#migrations)
    * [Views](#views)
    * [Mailers](#mailers)
    * [Bundler](#bundler)
* [Testing](#testing)
    * [RSpec](#rspec)
        * [Views](#rspec-views)
        * [Controllers](#rspec-controllers)
        * [Models](#rspec-models)
        * [Mailers](#rspec-mailers)
        * [Requests](#rspec-requests)
        * [Uploaders](#rspec-uploaders)

<a name='ruby'>
# Ruby

<a name='formatting'>
## Formatting
    * Formatting will not be enforced to the level of how many spaces or tabs to use when indenting your code, but is important to increasing the readability of your code.
    * Practices that increase scanning efficiency are favored over those that increase mental overhead in decifering the overall code structure.

<a name='syntax'>
## Syntax
  
<a name='language'>
## Know your language
  1. Enumerable
    This standard library is easily the most important one to understand. Most of our programming effort goes
    into gathering, organizing and manipulating collections of objects. This is the sole purpose of this
    library. If you find yourself constructing an iterator by hand, then you are probably doing it wrong.
  2. Hash
  3. Array
  4. Singleton
  5. Net/HTTP
  6. Forwardable/Delegate

<a name='naming'>
## Naming

<a name='comments'>
## Comments
  The best code is code that does not require comments. However, comments are helpful to explaining the purpose
  of classes and modules. Having a high-level understanding of the domain concept an object embodies is very
  helpful. We should strive to have documentation within all classes and modules that fulfills this role.

<a name='annotations'>
## Annotations
  1. Use TODO to note missing features that should be added at a later date.
  2. Use FIXME to note broken code that needs to be fixed.
  3. Use OPTIMIZE to note slow or inefficient code that may cause perfomance problems.
  4. Use HACK to note code smells where questionable coding practices were used and should be refactored away.
  5. Use REVIEW to note anything that should be looked at to confirm it is working as intended.

<a name='classes'>
## Classes
  * Avoid the usage of class (@@) variables due to their 'nasty' behavior in inheritance.
  * Use def self.method to define class methods. This makes the methods more resistant to refactoring changes.

<a name='exceptions'>
## Exceptions

<a name='collections'>
## Collections

<a name='strings'>
## Strings

<a name='literals'>
## Percent Literals

<a name='miscellaneous'>
## Miscellaneous

<a name='design'>
## Design

<a name='rails'>
# Rails

<a name='configuration'>
## Configuration

<a name='routing'>
## Routing

<a name='controllers'>
## Controllers

<a name='models'>
## Models

<a name='migrations'>
## Migrations

<a name='views'>
## Views

<a name='mailers'>
## Mailers

<a name='bundler'>
## Bundler

<a name='testing'>
# Testing

<a name='rspec'>
## RSpec

<a name='rspec-views'>
### Views

<a name='rspec-controllers'>
### Controllers

<a name='respec-models'>
### Models

<a name='rspec-mailers'>
### Mailers

<a name='rspec-requests'>
### Requests

<a name='rspec-uploaders'>
### Uploaders


