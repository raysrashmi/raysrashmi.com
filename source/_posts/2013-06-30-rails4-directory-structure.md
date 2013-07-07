---
title: Rails 4 Directory Structure
date: 30/06/2013
tags: rails4, rails
---
Rails 4 is out. We all are know about new features added in rails 4. What's new in our new rails app directory.
<br/>
We run following command to create new rails app:
<pre class="brush:ruby">
rails new [app name]
</pre>
<p>
Rails 4 has added couple of new directories and also removed few. 
Here we will see what new directories rails 4 added a why they been added.
</p>
<p>
We will create a sample app in rails 4 so that it will be easy to see the new structure.
</p>
<p>
I am assuming  you have updated your rails. 
To check current version of rails run following command:
<pre class ="brush:ruby">
rails -v
</pre>
It should be Rails 4.0.0
</p>
<p>
Now create a very simple rails application
<pre class="brush:ruby">
rails new  blog
</pre>
</p>
<p>
It creats bunch of files and directories. You will see there are some new directories and files.
Lets go to each one by one</p>
<ol>
<li class='title'> app/</li>
In this directory you will see new <b>concerns/</b> dir under controllers/ and models/.
<p>
Concerns are added to manage your models/controllers in a better way . You will get

1. Make your models ligh-weighted 
2. Cross sharing of modules code.

It means mixins and modules code can be put under this dir.

Lets say in our blog application we have user model and post model and we require to add commets for both model
so we created a commentable module in our concerns/ dir and included in both model.
<pre class ="brush:ruby">
  module CommentTable
    extend ActiveSupport::Concern
    def comments
      comments.map(&:text)
    end
  end
  class User < ActiveRecord::Base
    include Commentable
  end

  class Post < ActiveRecord::Base
    include Commentable
  end
</pre>
Now suppose we need to add one more module notable to our model post. 
<pre class="brush:ruby">
  module Noteable
    extend ActiveSupport::Concern
    included do
      has_many :notes
      validated :note, presence: true
    end
  end
  class Post < ActiveRecord::Base
    include Commentable
    include Noteable
  end
</pre>
Now you can include Commentable to Noteable
<pre class="brush:ruby">
  module Noteable
    extend ActiveSupport::Concern
    include Commentable
    included do
      has_many :notes
      validated :note, presence: true
    end
  end
  class Post < ActiveRecord::Base
    include Noteable
  end
</pre>
Now both Commentabel and Notebale are accesible for Post class.
</p>

<li class='title'> bin/</li>
App executable bundle, rails and rake are under this dir. So dont need to run 'bundle exec' before any rails command.
<a href='https://github.com/sstephenson/rbenv/wiki/Understanding-binstubs'>Binstubs</a> can also be generated using bundle command ,bundle 1.3 add spports for generating binstubs.
What Binstubs are exceutables required to prepare the enviroment original executable. 
<br/>
When we do 
<pre  class="brush:ruby">gem install cucmber</pre>
a excecutable file is genarte in gems//cucumber-1.3.2/bin dir(i am using rvm here)
<pre>~/.rvm/gems/ruby-1.9.3-p448/gems/cucumber-1.3.2/bin</pre>
<p>
Now we can generate using command
<pre  class="brush:ruby"> bundle binstubs cucumber</pre>
It wil genrate executale in bin/. Having executables within your app ensures they are using app ruby and gems.
</p>

<li class='title'>initializers/filter_parameter_logging.rb</li>
<p>
Now we can specify all filter parameters in this files instead of config/application.rb
Lets say we have a attribute customer_id of custome model and we want to filter in our log files then just write
</p>
<pre class="brush:ruby">
Rails.application.config.filter_parameters += [:customer_id]
</pre>
<p>
After retarting your server if there is any request having customer_id parameter. 
it will filtered the value of customer_id.
<br/>
<%= image_tag "filtered_params.png"%>
</p>

<li class='title' > doc/</li>
Doc has been removed from rails directory. 
Its is extracted in "sdoc" gem and is added to Gemfile by default so still you can generate docs by running following rake task

<pre class="brush:ruby">bundle exec rake doc:rails</pre>

<li class='title'> script/</li> Its removed and rails executable is moved to bin/ dir


<li class='title'> test/</li>
This directory has been changed differrntly. 
<ul>
  <li>Now we have controller and models in place of functionals and units</li>

  <li>We have helpers and mailers dir under test</li>

  <li>Performance test has been extracted to a new <a href='https://github.com/rails/rails-perftest'>Gem</a>. So perforance dir is not under test by default
  </li>
</ul>
Corresponding rake task for test are also changed -:
<pre>
rake test:controllers
rake test:models
rake test:helpers
rake test:mailers
</pre>

<li class='title'>vendor/</li> Rails plugins are already gone away so this dir is no more needed.
<li>scaffold</li>
<p>Our favorite rails command 'scaffold' gives us more new.
Lets create new scaffold
<pre class='"brush:ruby'>
rails generate scaffold list title:string 
</pre>
You will see there are two new files 
<div>index.json.builder</div>
<div>show.json.builder</div>
These files can be used for responding data in JSON format.
These are generated by <a href='https://github.com/rails/jbuilder'jbuilder></a><a href='http://rubygems.org/'>Gem</a>. It is included in Gemfile by default.
</p>
</ol>
<p>
TODO wrap up
</p>