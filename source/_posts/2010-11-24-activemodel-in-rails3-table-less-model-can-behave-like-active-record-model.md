---
title: ActiveModel in Rails3
date: 24/11/2010
tags: ActiveModel,Rails
---


In Rails3 Active Model feature is added that helps to use feature of active record class for a non active record class.In Active Model you can make Any Ruby Object Feel Like Active Record and can get Rails-style models with validations, serialization, callbacks, dirty tracking, internationalization, attributes, observers and all the other Rails goodness.

Here is the example -:
<pre class="brush:ruby">
class Message
  include ActiveModel::Validations
  include ActiveModel::Conversion
  include ActiveModel::Naming

  attr_accessor :name,:email,:text_message

  validates_presence_of :name
  validates_presence_of :email
  validates_presence_of :text_message
  validates_length_of :text_message, :maximum =&gt; 500

  def initialize(attributes={})
    attributes.each do |name, value|
      send("#{name}=", value)
    end
  end

end

</pre>
@message = Message.new({:name =&gt; "rays", :email =&gt; 'abc@example.com', :text_message =&gt; "this my text message"})

@message.valid? =&gt; true

@message = Message.new({:name =&gt; "rays", :text_message =&gt; "this my text message"})

@message.valid? =&gt; false

@message.errors will give error "Email can't be blank"