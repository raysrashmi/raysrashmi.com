---
layout: post
title: "rails-4-2-features"
date: 2014-08-28 15:42
comments: true
categories:
---
<a href='http://edgeguides.rubyonrails.org/4_2_release_notes.html'>Rails 4.2 beta</a> is releasing soon with some nice major features

1. <a href='http://edgeguides.rubyonrails.org/4_2_release_notes.html#active-job-action-mailer-deliver-later'>Active Job, ActionMailer #deliver_later</a>
2. <a href='http://edgeguides.rubyonrails.org/4_2_release_notes.html#adequate-record'>Adequate records</a>
3. <a href='http://edgeguides.rubyonrails.org/4_2_release_notes.html#web-console'>Web console</a>
4. <a href='http://edgeguides.rubyonrails.org/4_2_release_notes.html#foreign-key-support'>Foreign key support</a>

But part from this <a href='http://edgeguides.rubyonrails.org/4_2_release_notes.html'>Rails 4.2 beta</a> will give us other features which is good to know

<!--more-->
##`required` option for singular associations (`belongs_to` and `has_one`)
Current way of validating presence of association is -:

For example -:

```ruby
class User < ActiveRecord::Base
  belongs_to :account
  validates :account, presence: true
end
```
In Rails 4.2 we can set `required` option to true which will validate presence of association

```ruby
class User < ActiveRecord::Base
  belongs_to :account, required: true
end
```
We can also use it for `has_one` association

```ruby
class Account < ActiveRecord::Base
  has_one :user, required: true
end
```
##`required` option in migration and model generator

When we generate model we can pass `required` option  for references

```ruby
  rails generate model Comment commentable:references{required}
```

Model file will be generated like

```ruby
class Comment < ActiveRecord::Base
  belongs_to :commentable, required: true
end
```
commentable associatio is required here.

And migration file will be

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
It set `null: false` for references.

We can also pass it with `polymorphic` option

```ruby
rails generate model Comment commentable:references{required, polymorphic}

```

This `required` option can also be passed while generating migration

```ruby
rails generate migration AddAccountToUsers account:references{required}
```

##validate && validate!
###validate
It runs all validation and return `true` if no error found  otherwise return `false`.

It is alias of `valid?` method.

###validate!
It runs all validation and return `true` if no error found and
raise `ActiveRecord::RecordInvalid` if any validation fails

##`with_options` without explicit receiver
Now in Rails 4.2 we don't need to pass explicit receiver to `with_options` until it is required

```ruby
class User < ActiveRecord::Base
 with_options if: :is_admin? do
    validates :name, presence: true
    validates :email, presence: true
  end
end
```
## touch multiple attributes

You can update multiple attributes at once with `touch` method

```ruby
a = Article.first
a.touch(:published_at, :created_at)
```
##pretty_print support for ActiveRecord::Base objects
If you have active base record with many attributes and you want to print in console or in logs it just come in one line which is not so clear
Rails 4.2 pretty print support for active base record

```ruby
pp u
```
<img src="{{ root_url }}/images/pretty_print.png" />

## Skip gems

We can skip adding gems to Gem file while creating new app with `--skip-gems` option

```ruby
rails new <app name> --skip-gems turbolinks coffee-rails
```
## Empty your database
Rails provides us few rake tasks to create/delete database and to run migrations.

There is new task `rake db:purge` to empty database for current environment.

It removes your data and tables from database and of course we pass environment `RAILS_ENV` like any other rake task.


##Transform Hash Values
To modify Hash values call `tarnsform_values` it accepts a block and apply the block operation to each value of hash

```ruby
a = {a: 1, b: 2, c: 3}
a.transform_values{|a|a*2} #=> {:a=>2, :b=>4, :c=>6}
```
There is also bang version `transform_values!` which change original hash
```ruby
a = {a: 1, b: 2, c: 3}
a.transform_values!{|a|a*2} #=> {:a=>2, :b=>4, :c=>6}
a #=> {:a=>2, :b=>4, :c=>6}
```

##Truncate String by words
`truncate_words` method truncate a string by given number of words

```ruby
'This is fantastic place in this world'.truncate_words(3) #=> "This is fantastic..."
```
##bin/setup
We always need some set of commands to bootstrap our application.

Now in Rails 4.2 there is default `bin/setup` script where we can have all the commands required to bootstrap our application.

There are some defaults commands written but we can add more

Default bin/setup file look like

<img src="{{ root_url }}/images/bin_setup.png" />

##Support for PostgreSQL citext data type
Rails 4.2 added support for <a href='http://www.postgresql.org/docs/9.0/static/citext.html'>`citext`</a>
column type in PostgreSQL adapter.

You have to enable extension before running migration

`citext` column type provides a case-insensitive character string type.

```ruby
execute "CREATE EXTENSION IF NOT EXISTS citext"
```

For example we have a `users` table with bio column as `citext` type
```ruby
User.where(bio: 'Developer')
#=>SELECT "users".* FROM "users" WHERE "users"."bio" = $1  [["bio", "Developer"]]
#=> #<ActiveRecord::Relation [#<User id: 1, name: nil, email: "test@example.com", created_at: "2014-08-30 17:51:17", updated_at: "2014-08-30 17:51:17", role: nil, bio: "developer">]>
```
Here you can see we have user with bio value as 'developer' but when in `where` query with 'Developer' value gave us the user.

Internally `citext` calls lower when comparing values so we don't need to explicitly convert value in lowercase

#References

