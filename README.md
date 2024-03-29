# Mission
* We value the leverage that can be gained through the usage of common practices in all of our programming processes.
* Having practices that define a 'best' approach for solving a particular problem allows us to evaluate and improve our practices on a continual basis.
* What follows is a living document of the best practices for building applications at Revolution Prep.
* We acknowledge that this document is incomplete and may suggest practices that lose favor over time.
* We consider this document to be a best effort at defining the collective discussion of our programming practices.

# Core principles
  * [Follow best practices of object-oriented design and programming](#oop)
  * [Don't repeat yourself](#dry)
  * [Put business logic in code not database](#blnodb)
  * [Take simple performance considerations into account](#simple_perf)

## Table of Contents

* [Core Principles In Detail](#core_details)
* [Ruby](#ruby)
    * [Formatting](#formatting)
    * [Naming](#naming)
    * [Comments](#comments)
    * [Annotations](#annotations)
    * [Classes](#classes)
    * [Exceptions](#exceptions)
    * [Collections](#collections)
    * [Strings](#strings)
    * [Methods](#methods)
    * [Design](#design)
    * [Know Your Language](#language)
* [Rails](#rails)
    * [Configuration](#configuration)
    * [Routing](#routing)
    * [Controllers](#controllers)
    * [Models](#models)
        * [ActiveRecord](#activerecord)
    * [Migrations](#migrations)
    * [Views](#views)
    * [Mailers](#mailers)
    * [Bundler](#bundler)
* [Testing](#testing)
    * [RSpec](#rspec)
        * [Models](#rspec-models)
        * [Mailers](#rspec-mailers)
        * [Requests](#rspec-requests)
* [Javascript](#javascript)
  * [Object-Oriented JS](#oo-js)
  * [Functional JS](#functional-js)

<a name='core_details'>
# Core Principles In Detail

<a name='oop'>
## Follow best practices of object-oriented design and programming
* Use objects.
  * Avoid passing around unstructured data (e.g. deeply-nested hashes or arrays with specialized position information).
  * Instead, convert this data into objects and assign names to them. These names hopefully map to the domain concepts you are trying to model.
* Think through object models for new classes, using class diagrams if necessary.
  * After you have something that you think makes sense to you, find someone else to explain it to.
  * The point of object-oriented design is to make the human-overhead of understanding a codebase easier.
    If you can't explain the design to another developer, then you have missed the point.
* Try to make the object design reflect business reality as much as possible.
  * E.g., classes should not have a special "unassigned" section, just students not yet in a section.
* Use design patterns.
  * Even better, understand why the design pattern is a better solution to the specific problem you are trying to solve.
  * We aren't in academia. Most of the problems we face on a daily basis have been solved already.
* Make sure that each class only has code related to its own responsibilities.
  * Views should not query the database.
  * Models should not generate HTML.
  * Proctoring sessions should not grade exams.
* Tell, don't ask.
  * Objects should tell other objects to do something for them.
  * They should not ask an object for their state so that they can do something that should be the responsibility of that object.

<a name='dry'>
## Don't repeat yourself
* If you find yourself using the same code over and over, refactor it into a method or object.
  * Ruby is designed for objects. Having many small objects with unique responsibilities is how it is intended to be used.
  * Do not be afraid to create plain-old Ruby objects. These are the most powerful tools Ruby gives you to solve problems.
* If you find yourself finding objects using the same complex querying logic over and over, make a
[scope](#scopes).
  * This is one of the core design principles of Rails, so Rails makes this fairly easy to follow.

<a name='blnodb'>
## Put business logic in the code, not in the database
* No foreign key constraints
* No default values
* No triggers
* Columns should not allow nulls only if you are very sure there is never a business case for them to be null
* Any exceptions to this should be very carefully considered. Some that have been considered already include:
  * Ensuring the uniqueness of a column's values: Rails cannot ensure uniqueness through its validation. Uniqueness needs to be enforced at the database level. Use a unique index on that column.
  * Columns with default values: Default values can be enforced at the application-code level. Use
  [this](#default_values)
  to define them.

<a name='simple_perf'>
## Take simple performance considerations into account
While it's not a good idea to prematurely tune for performance, there are several easy things that everyone should stay aware of:

* Make sure that there are indexes on columns in MySQL tables that are involved in where clauses of queries.
* Don't do slow functions inside a loop if they can be moved out of it.
  * Use include if you know you will need all of the objects in a has_many relationship with the object you're fetching.
  * Make sure any API calls happen outside loops.
* Push off slow functions to the job servers using send_later.

<a name='ruby'>
# Ruby

<a name='formatting'>
## Formatting
* Formatting will not be enforced to the level of how many spaces or tabs to use when indenting your code, but is important for increasing the readability of your code.
* Practices that increase scanning efficiency are favored over those that increase mental overhead in deciphering the overall code structure.

<a name='naming'>
## Naming
* Use `snake_case` for methods and variables.
* Use `CamelCase` for classes and modules.  (Keep acronyms like HTTP,
  RFC, XML uppercase.)
* Use `SCREAMING_SNAKE_CASE` for other constants.
* The names of predicate methods (methods that return a boolean value)
  should end in a question mark.
  (i.e. `Array#empty?`).

<a name='comments'>
## Comments
  The best code is code that does not require comments. However, comments are helpful in explaining the purpose
  of classes and modules. Having a high-level understanding of the domain concept an object embodies is very
  helpful. We should strive to have documentation that fulfills this role within all classes and modules.

<a name='annotations'>
## Annotations
* Use `TODO` to note missing features or functionality that should be
  added at a later date.
* Use `FIXME` to note broken code that needs to be fixed.
* Use `OPTIMIZE` to note slow or inefficient code that may cause
  performance problems.
* Use `HACK` to note code smells where questionable coding practices
  were used and should be refactored away.
* Use `REVIEW` to note anything that should be looked at to confirm it
  is working as intended. For example: `REVIEW: Are we sure this is how the
  client does X currently?`

<a name='classes'>
## Classes
* Avoid the usage of class (@@) variables due to their 'nasty' behavior in inheritance.
* Use `def self.method` to define class methods. This makes the methods more resistant to refactoring changes.

<a name='exceptions'>
## Exceptions
* Don't suppress exceptions. This is especially important for our tests.  If your code is suppressing exceptions, bugs could slip by the test suite without causing failures.
* Don't use exceptions for flow of control.
* Avoid rescuing the `Exception` class.

<a name='collections'>
## Collections
* Use symbols instead of strings as hash keys.
* Avoid the use of mutable object as hash keys.
* Never modify a collection while traversing it.

<a name='strings'>
## Strings
* Prefer string interpolation instead of string concatenation:

    ```Ruby
    # bad
    email_with_name = user.name + ' <' + user.email + '>'

    # good
    email_with_name = "#{user.name} <#{user.email}>"
    ```

* Prefer single-quoted strings when you don't need string interpolation or
  special symbols such as `\t`, `\n`, `'`, etc.

    ```Ruby
    # bad
    name = "Bozhidar"

    # good
    name = 'Bozhidar'
    ```

* Avoid using `String#+` when you need to construct large data chunks.
  Instead, use `String#<<`. Concatenation mutates the string instance in-place
  and is always faster than `String#+`, which creates a bunch of new string objects.

    ```Ruby
    # good and also fast
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

<a name='methods'>
## Methods
* Avoid hashes as optional parameters. Does the method do too much?
* Avoid methods longer than 10 LOC (lines of code). Ideally, most methods will be shorter than
  5 LOC. Empty lines do not contribute to the relevant LOC.
* Avoid parameter lists longer than three or four parameters.

<a name='design'>
## Design
* Code in a functional way, avoiding mutation when that makes sense.
* Do not mutate arguments unless that is the purpose of the method.
* Do not mess around in core classes when writing libraries. (Do not monkey
  patch them.)
* [Do not program
  defensively.](http://www.erlang.se/doc/programming_rules.shtml#HDR11)
* Keep the code simple and subjective. Each method, and class should have a single,
  well-defined responsibility.
* Avoid more than three levels of block nesting.
* Don't overdesign. Overly complex solutions tend to be brittle and hard to
  maintain.
* Don't underdesign. A solution to a problem should be as simple as
  possible, but no simpler than that. Poor initial design can lead to a lot
  of problems in the future.
* Be consistent. In an ideal world, be consistent with these guidelines.
* Use common sense.

<a name='language'>
## Know your language

### [Enumerable](http://www.ruby-doc.org/core-1.8.7/Enumerable.html)
* This standard library is easily the most important one to understand. Most of our programming effort goes into gathering, organizing and manipulating collections of objects.
* This is the sole purpose of this library. If you find yourself constructing an iterator by hand, then you are probably doing it wrong.

### [Hash](http://www.ruby-doc.org/core-1.8.7/Hash.html)
* All developers should understand the following:
  * What the underlying implementation of Ruby's hash is.
  * How to define default values.
  * The difference between `hash[:key]` and `hash.fetch(:key)`.

### [Array](http://www.ruby-doc.org/core-1.8.7/Array.html)
* All developers should understand the following:
  * The difference between `array[index]` and `array.fetch(index)`
  * The performance implications of using Array operations instead of Set operations when operating on large collections.

### [Singleton](http://www.ruby-doc.org/stdlib-1.8.7/libdoc/singleton/rdoc/Singleton.html)
* It should be noted that the singleton is often an anti-pattern.
* Even worse, this standard library is probably the most ineffective way to implement such a design pattern.
* Instead, use modules with `extend self` or `module_function` to enable a much simpler interface.
* It should be restated the the singleton is often an anti-pattern.

### [Net/HTTP](http://www.ruby-doc.org/stdlib-1.8.7/libdoc/net/http/rdoc/Net/HTTP.html)
* This library uses a very strange style.
* When using it, wrap the default procedural calls in a reusable method, inside of a reusable class. Then, use this object in all other places that you would like to make HTTP requests.

### [Forwardable](http://www.ruby-doc.org/stdlib-1.8.7/libdoc/forwardable/rdoc/Forwardable.html)/[Delegate](http://www.ruby-doc.org/stdlib-1.8.7/libdoc/delegate/rdoc/)
* These libraries are helpful tools when it comes to implementing the delegator or interface patterns.
* Rails also has a `delegate` method that comes with [ActiveSupport](https://github.com/rails/rails/blob/1b819d32f6302e300da0188c4edb0f3b7bd48886/activesupport/lib/active_support/core_ext/module/delegation.rb)

<a name='rails'>
# Rails

<a name='configuration'>
## Configuration
* Put custom initialization code in `config/initializers`. The code in
  initializers executes on application startup.
* The initialization code for each gem should be in a separate file
  with the same name as the gem, for example `carrierwave.rb`,
  `rails_admin.rb`, etc.
* Initializers should read-in configuration data from YAML files that are not checked-in to the repository. This is to reduce configuration conflicts across environments.
* Adjust accordingly the settings for development, test and production
  environment (in the corresponding files under `config/environments/`)

<a name='routing'>
## Routing
* When you need to add more actions to a RESTful resource (do you
  really need them at all?) use `member` and `collection` routes.

    ```Ruby
    # bad
    get 'subscriptions/:id/unsubscribe'
    resources :subscriptions

    # good
    resources :subscriptions do
      get 'unsubscribe', :on => :member
    end

    # bad
    get 'photos/search'
    resources :photos

    # good
    resources :photos do
      get 'search', :on => :collection
    end
    ```

* If you need to define multiple `member/collection` routes use the
  alternative block syntax.

    ```Ruby
    resources :subscriptions do
      member do
        get 'unsubscribe'
        # more routes
      end
    end

    resources :photos do
      collection do
        get 'search'
        # more routes
      end
    end
    ```

* Use nested routes to express better the relationship between
  ActiveRecord models.

    ```Ruby
    class Post < ActiveRecord::Base
      has_many :comments
    end

    class Comments < ActiveRecord::Base
      belongs_to :post
    end

    # routes.rb
    resources :posts do
      resources :comments
    end
    ```
* Avoid nesting routes more than two levels deep.

* Use namespaced routes to group related actions.

    ```Ruby
    namespace :admin do
      # Directs /admin/products/* to Admin::ProductsController
      # (app/controllers/admin/products_controller.rb)
      resources :products
    end
    ```

* Never use the legacy wild controller route. This route will make all
  actions in every controller accessible via GET requests.

    ```Ruby
    # very bad
    match ':controller(/:action(/:id(.:format)))'
    ```

<a name='controllers'>
## Controllers
* Keep the controllers skinny - they should only retrieve data for the view layer and shouldn't contain any business logic (all the business logic should naturally reside in the model).

<a name='models'>
## Models
* Introduce non-ActiveRecord model classes freely.
* Models should map to the domain concepts, not the tables in the database.
  * Tables in a database are an implementation detail for the purpose of persistence and should not guide the domain modeling process.
  * Try implementing the domain concept with pure Ruby objects and then introducing persistence later. It will change the way you build models.
  * [Food for thought](http://solnic.eu/2011/08/01/making-activerecord-models-thin.html)
* Name the models with meaningful (but short) names without abbreviations.

<a name='activerecord'>
### ActiveRecord
* Avoid altering ActiveRecord defaults (table names, primary key, etc)
  unless you have a very good reason (like a database that's not under
  your control).
* Group macro-style methods (`has_many`, `validates`, etc) in the
  beginning of the class definition.
* Always use the new
  ["sexy" validations](http://thelucid.com/2010/01/08/sexy-validation-in-edge-rails-rails-3/).
* When a custom validation is used more than once or the validation is some regular expression mapping, create a custom validator file.

    ```Ruby
    # bad
    class Person
      validates :email, format: { with: /^([^@\s]+)@((?:[-a-z0-9]+\.)+[a-z]{2,})$/i }
    end

    # good
    class EmailValidator < ActiveModel::EachValidator
      def validate_each(record, attribute, value)
        record.errors[attribute] << (options[:message] || 'is not a valid email') unless value =~ /^([^@\s]+)@((?:[-a-z0-9]+\.)+[a-z]{2,})$/i
      end
    end


    class Person
      validates :email, email: true
    end
    ```

* Use named scopes freely.
<a name='scopes' >
* When a named scope, defined with a lambda and parameters, becomes too
complicated it is preferable to make a class method instead which serves
the same purpose of the named scope and returns and
`ActiveRecord::Relation` object.

    ```Ruby
    # complex
    class Student
      scope :with_upcoming_meetings,  joins('LEFT JOIN `enrollments` ON `student`.`id` = `enrollments`.`student_id`').
                                      joins('LEFT JOIN `meetings` ON `enrollments`.`course_id` = `meetings`.`course_id`').
                                      where('`meetings`.`starts_at` > NOW() AND `meetings`.`starts_at` < DATE_ADD(NOW(), INTERVAL 1 DAY)').
                                      group('`student`.`user_id`')
    end

    # simpler
    class Student

      def self.with_meetings_in_last_day
        joins('LEFT JOIN `enrollments` ON `student`.`id` = `enrollments`.`student_id`').
        joins('LEFT JOIN `meetings` ON `enrollments`.`course_id` = `meetings`.`course_id`').
        where('`meetings`.`starts_at` > NOW() AND `meetings`.`starts_at` < DATE_ADD(NOW(), INTERVAL 1 DAY)')
      end

      scope :by_user_id, group('`students`.`user_id`')
      scope :with_upcoming_meetings, with_meetings_in_last_day.by_user_id

    end
    ```

* Beware of the behavior of the `update_attribute` method. It doesn't
  run the model validations (unlike `update_attributes`) and could easily corrupt the model state.

* Avoid callbacks such as `before_create` and `after_save`.
  * Instead, try wrapping the domain concept in a higher-level model that makes it its responsibility to ensure these actions occur at the right times.
  * It is more clear.
  * It avoids possible infinite-loop or callback ordering issues.
  * It reduces the responsibilities of the class.
  * It reduces coupling.

<a name='migrations'>
## Migrations
* Keep the `schema.rb` under version control.
* Use `rake db:schema:load` instead of `rake db:migrate` to initialize
  an empty database.
<a name='default_values'>
* Avoid setting defaults in the tables themselves. Use the model layer
  instead.

    ```Ruby
    def amount
      read_attribute(:amount) || 0
    end
    ```

<a name='views'>
## Views
* Never call the model layer directly from a view.
* Never make complex formatting in the views, export the formatting to
  a method in a view helper or a decorator.
  * Understand the reasons to use a decorator or helper.
  * Decorators are much better at presentation logic associated with a domain model.
  * Helpers are much better at object-agnostic presentation concepts.
  * [Draper](https://github.com/jcasimir/draper) is a pretty good decorator library.
* Mitigate code duplication by using partial templates and layouts.

<a name='mailers'>
## Mailers
* Name the mailers `SomethingMailer`. Without the Mailer suffix it
  isn't immediately apparent what's a mailer and which views are
  related to the mailer.
* Provide both HTML and plain-text view templates.
* Enable errors raised on failed mail delivery in your development environment. The errors are disabled by default.

    ```Ruby
    # config/environments/development.rb

    config.action_mailer.raise_delivery_errors = true
    ```

* Use `smtp.gmail.com` for SMTP server in the development environment
  (unless you have local SMTP server, of course).

    ```Ruby
    # config/environments/development.rb

    config.action_mailer.smtp_settings = {
      address: 'smtp.gmail.com',
      # more settings
    }
    ```

* Provide default settings for the host name.

    ```Ruby
    # config/environments/development.rb
    config.action_mailer.default_url_options = {host: "#{local_ip}:3000"}


    # config/environments/production.rb
    config.action_mailer.default_url_options = {host: 'your_site.com'}

    # in your mailer class
    default_url_options[:host] = 'your_site.com'
    ```

* If you need to use a link to your site in an email, always use the
  `_url`, not `_path` methods. The `_url` methods include the host
  name and the `_path` methods don't.

    ```Ruby
    # wrong
    You can always find more info about this course
    = link_to 'here', url_for(course_path(@course))

    # right
    You can always find more info about this course
    = link_to 'here', url_for(course_url(@course))
    ```

* Format the from and to addresses properly. Use the following format:

    ```Ruby
    # in your mailer class
    default from: 'Your Name <info@your_site.com>'
    ```

* Make sure that the e-mail delivery method for your test environment is set to `test`:

    ```Ruby
    # config/environments/test.rb

    config.action_mailer.delivery_method = :test
    ```

* The delivery method for development and production should be `smtp`:

    ```Ruby
    # config/environments/development.rb, config/environments/production.rb

    config.action_mailer.delivery_method = :smtp
    ```

<a name='bundler'>
## Bundler
* Put gems used only for development or testing in the appropriate group in the Gemfile.
* Use only established gems in your projects. If you're contemplating
on including some little-known gem you should do a careful review of
its source code first.
* Do not remove the `Gemfile.lock` from version control. This is not
  some randomly generated file - it makes sure that all of your team
  members get the same gem versions when they do a `bundle install`.

<a name='testing'>
# Testing

<a name='rspec'>
## RSpec
* Strive to have specs that fail gracefully, with output that would be relevent to the person debugging the issue.
  * An easy way to ensure relevent error messages is to write specs that only have one assertion.
  * This is not dogma. The key is to have specs that tell you why they failed in a way that is easy to understand.
  * You could write a custom matcher that accomplishes this same goal.

    ```Ruby
    # bad
    describe ArticlesController do
      #...

      describe 'GET new' do
        it 'assigns new article and renders the new article template' do
          get :new
          assigns[:article].should be_a_new Article
          response.should render_template :new
        end
      end

      # ...
    end

    # good
    describe ArticlesController do
      #...

      describe 'GET new' do
        it 'assigns a new article' do
          get :new
          assigns[:article].should be_a_new Article
        end

        it 'renders the new article template' do
          get :new
          response.should render_template :new
        end
      end

    end
    ```

* Make heavy use of `describe` and `context`
* Name the `describe` blocks as follows:
  * use "description" for non-methods
  * use pound "#method" for instance methods
  * use dot ".method" for class methods

    ```Ruby
    class Article
      def summary
        #...
      end

      def self.latest
        #...
      end
    end

    # the spec...
    describe Article
      describe '#summary'
        #...
      end

      describe '.latest'
        #...
      end
    end
    ```

* Use [factories](https://github.com/thoughtbot/factory_girl) to create test objects.

<a name='rspec-models'>
### Models
* Do not mock the models in their own specs.
* Use [factories](https://github.com/thoughtbot/factory_girl) to create test objects.
* It is acceptable to mock other models or child objects.

<a name='rspec-mailers'>
### Mailers
* The model in the mailer spec should be mocked. The mailer should not depend on the model creation.
* The mailer spec should verify that:
  * the subject is correct
  * the receiver e-mail is correct
  * the e-mail is sent to the right e-mail address
  * the e-mail contains the required information

     ```Ruby
     describe SubscriberMailer
       let(:subscriber) { mock_model(Subscription, email: 'johndoe@test.com', name: 'John Doe') }

       describe 'successful registration email'
         subject { SubscriptionMailer.successful_registration_email(subscriber) }

         its(:subject) { should == 'Successful Registration!' }
         its(:from) { should == ['info@your_site.com'] }
         its(:to) { should == [subscriber.email] }

         it 'contains the subscriber name' do
           subject.body.encoded.should match(subscriber.name)
         end
       end
     end
     ```

<a name='rspec-requests'>
### Requests

<a name='javascript'>
# Javascript

<a name='oo-js'>
## Object-Oriented JS
* Javascript has a very powerful object-oriented model.
* Learn how to use this to namespace your functions.
* [Here](http://www.klauskomenda.com/code/javascript-programming-patterns/) are several patterns that make working with Javascript more like other OO languages.

<a name='functional-js'>
## Functional JS
* Javascript's functional programming paradigms are what makes it truly powerful.
* You should understand what [higher-order functions](http://en.wikipedia.org/wiki/Higher-order_function),
[callbacks](http://recurial.com/programming/understanding-callback-functions-in-javascript/),
and [closures](http://robertnyman.com/2008/10/09/explaining-javascript-scope-and-closures/)
are and how and when to use them.
