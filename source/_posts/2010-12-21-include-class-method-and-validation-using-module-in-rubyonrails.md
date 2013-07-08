---
layout: post
title: Include Class Methods and Validation Using Module in RubyOnRails
date: 21/12/2010
tags: Modules,Rails
---

{% codeblock lang:ruby%}
class User
  include TestModule
end

module TestModule
  def test_instance_method
    puts "Test Method"
  end

  def self.included(base)
    base.extend ClassMethods
  end

  module ClassMethods
    def my_class_method
      puts "Class Methods"
    end
  end
end
{% endcodeblock %}


Here my_class_method is Class Method.we can define more class methods in module ClassMethods

Validations and call backs are called without prefix 'self'. so for validations and call backs
we have to explicitly set the class


{% codeblock lang:ruby%}
module TestModule
  def self.included(base)
    base.validates_presence_of :bar
    base.before_save :some_method
  end
end
{% endcodeblock %}

