---
layout: post
title: "Rails 4.2 beta Changelog"
date: 2014-08-28 15:42
comments: true
categories:
keywords: rails 4.2 beta, rails changelog rails, ruby
---

<a href='http://edgeguides.rubyonrails.org/4_2_release_notes.html'>Rails 4.2 beta</a> has been released and final version is going to come out soon.

Listed are some feature are going introduced in version of Rails 4.2. You can also try them while it's beta.

1. <a href='http://edgeguides.rubyonrails.org/4_2_release_notes.html#active-job-action-mailer-deliver-later'>Active Job, ActionMailer #deliver_later</a>

2. <a href='http://edgeguides.rubyonrails.org/4_2_release_notes.html#adequate-record'>Adequate records</a>

3. <a href='http://edgeguides.rubyonrails.org/4_2_release_notes.html#web-console'>Web console</a>

4. <a href='http://edgeguides.rubyonrails.org/4_2_release_notes.html#foreign-key-support'>Foreign key support</a>

But apart from these there are more features which are going to ship in Rails 4.2.

Few of them are..

<!--more-->
##`required` option for singular associations (`belongs_to` and `has_one`)
Sometime we need to validate presence of associated object not the foreign key used to map the association

Lets say for each user there must be a account so we validate it like this -:

```ruby
class User < ActiveRecord::Base
  belongs_to :account
  validates :account, presence: true
end
```
But in Rails 4.2 we just need to set `required` option to true which will validate associated object is present

```ruby
class User < ActiveRecord::Base
  belongs_to :account, required: true
  has_one :profile, required: true
end
```

##`required` option in generator for model and migration

When we generate model we can pass `required` option  for references/associations

```ruby
  rails generate model Comment commentable:references{required}
```

It will automatically set `required` true for `commentable` association

```ruby
class Comment < ActiveRecord::Base
  belongs_to :commentable, required: true
end
```

And in migration set `null: false` for `commentable` association which is basically a database validation

```ruby
class CreateComments < ActiveRecord::Migration
  def change
    create_table :comments do |t|
      t.references :commentable, index: true, null: false

      t.timestamps null: false
    end
  end
end
```

We can also pass it with `polymorphic` option if we need polymorphic relation

```ruby
rails generate model Comment commentable:references{required, polymorphic}

```

This `required` option can also be passed in generating migration

```ruby
rails generate migration AddAccountToUsers account:references{required}
```

##validate and validate!
###validate
It runs all validations and returns `true` if no error found  otherwise return `false`.

It is alias of `valid?` method.

###validate!
It runs all validations and returns `true` if no error found otherwise
raise `ActiveRecord::RecordInvalid` if any validation fails

##`with_options` without explicit receiver
In `with_options` block we have to pass explicit receiver whether it is required or not

```ruby
class User < ActiveRecord::Base
 with_options if: :is_admin? do |admin|
    admin.validates :name, presence: true
    admin.validates :email, presence: true
  end
end
```

Now in Rails 4.2 we don't need to pass explicit receiver to `with_options` until it is required

```ruby
class User < ActiveRecord::Base
 with_options if: :is_admin? do
    validates :name, presence: true
    validates :email, presence: true
  end
end
```
## Touch multiple attributes
Before Rails 4.2 we can just pass one attribute in `touch` method
```ruby
a = Article.first
a.touch(:published_at) # => true
a.touch(:published_at, :created_at) #=> ArgumentError: wrong number of arguments (2 for 0..1)
```
But in Rails 4.2 we can update multiple attributes at once with `touch` method

```ruby
a = Article.first
a.touch(:published_at, :created_at) #=> true
```
##Support for PostgreSQL citext data type
Rails 4.2 added support for <a href='http://www.postgresql.org/docs/9.0/static/citext.html'>`citext`</a>
column type in PostgreSQL adapter.

But we have to enable extension before running migration because its additional supplied module

```ruby
execute "CREATE EXTENSION IF NOT EXISTS citext"
```

`citext` column type provides a case-insensitive character string type.

For example we have a `users` table with bio column as `citext` type

We have a user object with bio value as 'developer' but we search with 'Developer' it will return our user object in result

```ruby
User.where(bio: 'Developer')
#=>SELECT "users".* FROM "users" WHERE "users"."bio" = $1  [["bio", "Developer"]]
#=> #<ActiveRecord::Relation [#<User id: 1, name: nil, email: "test@example.com", created_at: "2014-08-30 17:51:17", updated_at: "2014-08-30 17:51:17", role: nil, bio: "developer">]>
```

Internally `citext` calls lower when comparing values so we don't need to explicitly convert value in lowercase

```ruby
SELECT * FROM tab WHERE lower(col) = LOWER(?);
```
## Empty your database
Rails provides us few database related rake tasks such as to create/drop database and to run migrations.

There is a new rake task added `rake db:purge` to empty database for current environment.

It removes your data and tables from database and of course we can pass environment `RAILS_ENV` like any other rake task.

##New binstubs(bin/setup)
We always need some set of commands to bootstrap our application.

Now in Rails 4.2 there is default `bin/setup` script where we can have all tasks to setup our application in quick and consistent way. It is located in bin directory.

There are some defaults commands given by Rails but we can add more as per our requirement.

##Transform Hash Values
To modify Hash values call `tarnsform_values` it accepts a block and apply the block operation to each value of hash

```ruby
a = {a: 1, b: 2, c: 3}
a.transform_values{ |a|a*2 } #=> {:a=>2, :b=>4, :c=>6}
```
There is also a bang version `transform_values!` which change original hash
```ruby
a = {a: 1, b: 2, c: 3}
a.transform_values!{ |a|a*2 } #=> {:a=>2, :b=>4, :c=>6}
a #=> {:a=>2, :b=>4, :c=>6}
```

##Truncate String by words
There is new method `truncate_words` which truncate a string by given number of words length

```ruby
'This is fantastic place in this world'.truncate_words(3) #=> "This is fantastic..."
```

##Pretty print for ActiveRecord object
Now in Rails console/logs we can print ActiveRecord object output nicely
just pass that object to `pp` method

```ruby
pp User.last

#<User:0x007f8fdf29f3a8
 id: 2,
 name: nil,
 email: nil,
 created_at: Tue, 02 Sep 2014 19:18:52 UTC +00:00,
 updated_at: Tue, 02 Sep 2014 19:18:52 UTC +00:00,
 role: "user",
 bio: nil>
```
## Skip gems

We can skip default gems to Gem file while creating new app with `--skip-gems` option. They will not be added to our `Gemfile`

```ruby
rails new <app name> --skip-gems turbolinks coffee-rails
```

###References
Here are some more articles about rails 4.2

* http://edgeguides.rubyonrails.org/4_2_release_notes.html
* http://blog.newrelic.com/2014/04/25/ponies-rails-adequaterecord/
* http://blog.plataformatec.com.br/2014/07/the-new-html-sanitizer-in-rails-4-2/
* http://www.justinweiss.com/blog/2014/08/25/the-lesser-known-features-in-rails-4-dot-2/

Thanks to all Rails contributors for making Rails awesome :)
