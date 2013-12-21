---
layout: post
title: splat operator as an argument in ruby
date: 06/11/2013
tags: ruby
description: splat operator as an argument in ruby
keywords: ruby, splat operator, asterisk, rails
---
As we know we see `*args` as an argument in method definition it means we can pass
multiple parameters.
See the below example


```ruby
clas Test
  def initialize(*args)
    puts args
  end
end
Test.new('a', 'b') =>
a
b
```

But what if we just have asterisk(splat operator) as an argument?

I was looking Rails code base and found  asterisk`(*)` in an class initialize method.

Link: https://github.com/rails/rails/blob/master/railties/lib/rails/commands/server.rb#L42

Let's take an example -:

```ruby
def hello(a, *)
  puts a
end
hello('rays', 'b')
rays
```

How many arguments you passed it wont generate any error

Let's take another example

```ruby
class A
  def initialize(argument=nil)
    puts "#{argument}"
    puts 'I am in Parent class'
  end
end

class B < A
  def initialize()
   super
   puts 'I am in Child class'
  end
end

b = B.new #=> 
I am in Parent class
I am in Child class

B.new('a') #=>
`initialize': wrong number of arguments (1 for 0) (ArgumentError)
```

But now if we pass `*` as an argument in child class initialize method see what
happens.

```ruby
class A
  def initialize(argument=nil)
    puts "#{argument}"
    puts 'I am in Parent class'
  end
end

class B < A
  def initialize(*)
   super
   puts 'I am in Child class'
  end
end

B.new #=> 

I am in Parent class
I am in Child class

B.new('a') #=>

a
I am in Parent class
I am in Child class
```
You can see passing * as an argument help to send parameters for parent class even though child class doesn't require those arguments

Note: `*` is called splat operator in Ruby.


See some sample Rails code which is using `*`

https://github.com/rails/rails/blob/v4.0.0/railties/lib/rails/commands/server.rb#L40
https://github.com/rack/rack/blob/1.5.2/lib/rack/server.rb#L178

It was new to me so thought to share with you guys. I hope you can get it if you do not know about this already.
