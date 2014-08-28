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

##`required` option for singular associations (`belongs_to` and `has_one`)
Sometimes we want to have presence validation for association

For example -:

```ruby
class User < ActiveRecord::Base
  belongs_to :account
  validates :account, presence: true
end
```
In rails 4.2 we can set option `required` true which will validate association presence
```ruby
class User < ActiveRecord::Base
  belongs_to :account, required: true
end
```
Also with `has_one` association we can set `required` true
We can set `required` true for `has_one` as well

```ruby
class Account < ActiveRecord::Base
  has_one :user, required: true
end
```
##`required` option in migration and model generator

When we generate model we can pass `required` option

```ruby
  rails generate model Comment commentable:references{required}
```

Model file will be generated like

```ruby
class Comment < ActiveRecord::Base
  belongs_to :commentable, required: true
end
```
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
It runs all validation and return `true` if no error found  return `false` if
alias of `valid?`

###validate!
Its run all validation return `true` if no error found and
raise `ActiveRecord::RecordInvalid` if validation fails

##pretty_print support for ActiveRecord::Base objects
If you have active base record with many attributes and you want to print in console or in logs it just come in one line which looks messy kind of
Rails 4.2 pretty print support for active base record

```ruby
pp u
```
```ruby
<%= image_tag %>
```


