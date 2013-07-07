---
title: Enhance Rails Models
date: 05/05/2012
tags: activerecord,association,polymorphic 
---
In Rails Models play a important part in our Rails App.We should keep are model pretty,not having dirty code
Here i am just reminding you some basic points to refactor a model.

READMORE

<p>
  <strong>1. Follow law of Demeter(In rails it means use only one dot)</strong>

<pre class="brush:ruby">
class Author  < ActiveRecord::Base
  has_many :books
  has_one  :address
end

class Book < ActiveRecord::Base
  belongs_to :author
end

class Address < ActiveRecord::Base
  belongs_to :author
end
</pre>
  <p>So if in a view we have something like</p>
  <pre class="brush:ruby">
    @book.author.name
    @book.author.address.street
    @book.author.address.city
    @book.author.address.state
  </pre>
  Here we are calling an object's related object using a third(book -> customer -> address)

  Luckily in Rails we have Delegate that can escape us from this situation

<pre class="brush:ruby">
class Author  < ActiveRecord::Base
  has_many :books
  has_one  :address
end

class Book < ActiveRecord::Base
  belongs_to :author'
  delegate :name,
           :street,
           :city,
           :state,
           :to => :author, :prefix => true, :allow_nil => true

end

class Address < ActiveRecord::Base
  belongs_to :author
  delegate :street,:city,:state, :to => :address ,:allow_nil => true
end

</pre>
<p>So if in a view we have something like</p>
<pre class="brush:ruby">
  @book.author_name
  @book.author_street
  @book.author_city
  @book.author_state
</pre>
<p>Now we have just only dot.Here :allow_nil option prevents the error call method on nil object</p>

</p>
<p>
<strong>2. Use callback and validation.instead of writing large code in your method</strong>

<pre class="brush:ruby">
class User < ActiveRecord::Base
  email_confirmation
  attr_accessor :email_confirmation

  def user_save
    if email && email_confirmation && save
     #send_email
    end
  end
end
</pre>

Instead of doing that you can use a call back after_create
<pre class="brush:ruby">
class User < ActiveRecord::Base
  validates :email, :confirmation => true
  validates :email_confirmation, :presence => true

  after_create :send_email

  private

  def send_email
   #send_email
  end
end
</pre>

So now when you create user it validates if email and email_confirmation attribute is there will send email just after saving the user
(here you don't need to define attr_accessor :email_confirmation and don't need to tae attr email_confirmation in db table )
</p>
<p>
<strong>3. Include Modules</strong>
<pre class="brush:ruby">
class Order < ActiveRecord::Base
  def self.find_purchased
    # ...
  end

  def self.find_waiting_for_review
     # ...
  end

  def self.find_waiting_for_sign_off
    #.....
  end

  def self.find_waiting_for_sign_off
   #...
  end

  def self.advanced_search(fields, options = {})
   #...
  end

  def self.simple_search(terms)
   #...
  end

  def to_xml
   #...
  end

  def to_json
   #...
  end

  def to_csv
    #...
  end

  def to_pdf
    #...
  end
end
</pre>

Modules allow you to extract behavior into separate files.Modules also serve to group related information into labeled namespaces.Its improve readability of code
<pre class="brush:ruby">
class Order < ActiveRecord::Base
  extend OrderStateFinders
  extend OrderSearchers
  include OrderExporters
end

# lib/order_state_finders.rb
module OrderStateFinders
  def find_purchased
    #...
  end

  def find_waiting_for_review
    #...
  end

  def find_waiting_for_sign_off
    #...
  end
end

# lib/order_searchers.rb
module OrderSearchers
  def advanced_search(fields, options = {})
    # ...
  end

  def simple_search(terms)
    # ...
  end
end

# lib/order_exporters.rb
module OrderExporters
  def to_xml
    # ...
  end

  def to_json
    # ...
  end

  def to_csv
   # ...
  end

  def to_pdf
    # ...
  end
end
</pre>

So in extend module's method are class method on that calling class.include module's methods are instance method for object of calling class

</p>
<p>
<strong>4.Use active record association</strong>

Rails provides us association very nicely.
<pre class="brush:ruby">
class User < ActiveRecord::Base
  has_many :blogs
end

class blog < ActiveRecord::Base
  belongs_to :user
end


@user = User.find(params[:id])
@blogs = Blog.where(:user_id => params[:user_id])
</pre>

<p>Instead of doing this.do</p>

<pre>
@user = User.find(params[:id])
@blogs = @user.blogs
</pre>
</p>
<p>
<strong>5. Use Scope rather than writing complex finders</strong>
<pre class="brush:ruby">
class Book < ActiveRecord::Base
  def search_books(params={})
    where('name like ? and price > ? and published_date > =?', "%#{params[:name]}%", 100,Date.today)
  end
end
</pre>

Now suppose if you just need to find books based on published date you will create another method.That will duplicate your code
<pre class="brush:ruby">
class Book < ActiveRecord::Base
  def search_books(params={})
    where('name like ? and price > ? and published_date > =?', "%#{params[:name]}%", 100,Date.today)
  end

  def search_by_published_date(date)
    where('published_date > =?',date)
  end
end
</pre>

In this case you can use scope
<pre class="brush:ruby">
class Book < ActiveRecord::Base
  scope :by_published_date,lambda{|d|{:where => ['published_date >= ?',d]}}
  scope :by_name,lambda{|n|{:where => ['name like ?',"%#{n}%"]}}
  scope :by_price,lambda{|p|{:where => ['price =?',p}}
end
</pre>

Now you can use scope like
<pre class="brush:ruby">
Book.by_published_date(Date.today).by_name('rails').by_price(100)
</pre>
</p>
<p>
<strong>6. Do Metaprogramming</strong>

<pre class="brush:ruby">
class Plan < ActiveRecord::Base
  ACTIVE='active'
  INACTIVE = 'inactive'
  IN_PROGRESS = 'in progress'
  def active?
    status == ACTIVE
  end

  def inactive?
    status == INACTIVE
  end

  def in_progress?
    status == IN_PROGRESS
  end
end
</pre>
now reduce code with meta programming
<pre class="brush:ruby">
class Plan < ActiveRecord::Base
  ACTIVE='active'
  INACTIVE = 'inactive'
  IN_PROGRESS = 'in progress'

  [ACTIVE, INACTIVE,IN_PROGRESS].each do |s|
    plan_status = <<-EOF
      def is_#{s}?
        self.status ==  '#{s}'
      end
    EOF
    class_eval plan_status, __FILE__, __LINE__
  end
end
</pre>
</p>
<p>
<strong>7. Use full text search engine</strong>
  <p>
if you app require search based on all the attribute on model or something like that then writing messy code is not an good idea.
Better way is to use any search engine like Solr, Sphinx
 </p>

<p>Search engine indexed the data.so it gives you results based on search query against any full words in the indexed data</p>
</p>
<p>
  I have covered most of the steps here to enhance your Rails Models and keep your Rails Models clean,
  Here are some links to read more about these things.

  Reference links :
</p>
<ul>
  <li><a href="http://blog.rubybestpractices.com/posts/gregory/008-decorator-delegator-disco.html">http://blog.rubybestpractices.com/posts/gregory/008-decorator-delegator-disco.html</a></li>
  <li><a href="http://khelll.com/blog/ruby/delegation-in-ruby/">http://khelll.com/blog/ruby/delegation-in-ruby/</a></li>
  <li><a href="http://sunspot.github.com/">http://sunspot.github.com/</a></li>
  <li><a href="http://freelancing-god.github.com/ts/en/">http://freelancing-god.github.com/ts/en/</a></li>
</ul>

Thanks for reading this post :-)
