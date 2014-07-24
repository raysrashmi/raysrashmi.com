---
layout: post
title: Rails Routing Constraints
tags: routes, rails, ruby
description: route constraints
keywords: routes, constraints, rails, ruby
---
Routing is backbone of a web applications. When we are building applications using Rails we can manage routing for our app easily. I am going to explain a bit advance feature of routing in Rails

##What is routing constraints
Few many times we require routes to behave differently and also want to do some routing based on specific conditions.

 For example

  *  Constraint routes for http methods( put, post )

  * Want to show different home page for different users.

  * Want to restrict URL for some sub domain or range of IP addresses

We can easily constraints our routes in Rails


##How many ways we can constraint our routes

### Segment Constraints(Parameter Restriction)

You can filter any parameter based on Regular expression
<!--more-->

Lets say we have route

```ruby
resources :maps
```

If we want to restrict it for certain range of IP addresses we can do like

```ruby
    resources :maps, constraints: { ip: /172\.124\.\d+\.\d+/ }
```
OR

```ruby
    constraints( ip: /172\.124\.\d+\.\d+/) do
      resources :maps
    end
```

Another example of filtering `id` parameter

```ruby
    match 'maps/:id', to: 'maps#show' ,constraints: { id: /\d+/}, via: :get
```

Now URL with `id` as integer only allowed to hit `maps#show` action

Can restrict `format` parameter

```ruby
    match 'maps/:id', to: 'maps#show' ,constraints: { format: 'json' }, via: :get
```

It will generate route only for `json` format so if somebody try to open HTML format for this URL it won't hit maps show action.

### Request-Based Constraints
We can also constraint routes on request object

```ruby
    get 'admin/', to: 'admin#show', constraints: { subdomain: 'admin' }
```

Now URL `admin.example.com/admin` will redirect to `show` action of `admin`
controller

### Dynamic Constraint(Dynamic request matching)

  * Using `matches?` method

We can constraint route dynamically based on some specific criteria by creating a `matches?` method

Lets say we have to filter sub domain of URL

```ruby
    constraints Subdomain do
      get '*path', to: 'proxy#index'
    end

    class Subdomain
      def self.matches?(request)
        (request.subdomain.present? && request.subdomain.start_with?('foobar')
      end
    end
```
What we are doing here is checking for URL if it start with sub domain `foobar` then only hit `proxy#index` action. Your class must have `mathes?` method either class method or instance method. If you want to make it a instance method then do

```ruby
    constraints Subdomain.new do
      get '*path', to: 'proxy#index'
    end
```

  * Using `lambdas`

Instead of writing class we can also use `lambdas`
```ruby
    get '*path', to: 'proxy#index', constraints: lambda{|request|request.env['SERVER_NAME'].match('foo.bar')}
```

Resource

 * http://guides.rubyonrails.org/routing.html
