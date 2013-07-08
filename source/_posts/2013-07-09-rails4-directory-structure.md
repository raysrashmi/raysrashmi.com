---
layout: post
title: Rails 4 Directory Structure
date: 30/06/2013
tags: rails4, rails
---
<a href='http://farm6.staticflickr.com/5443/9241313505_ac035bd643_b.jpg'
target="_blank">
{%img http://farm6.staticflickr.com/5443/9241313505_ac035bd643_m_d.jpg%}
</a>

Rails 4 is released. Lots of new features has been added to rails.
Here we will see what and why new directories or files rails 4 added.

  We will create a sample app in rails 4 so that it will be easy to see the new structure.
  I am assuming  you have updated your rails.
  To check current version of rails run following command:
  {% codeblock lang:ruby %}rails -v{%endcodeblock%}
  It should be Rails 4.0.0

<!--more-->

  Now create a very simple rails application
  {% codeblock lang:ruby %}rails new  blog  {% endcodeblock %}

  It creates bunch of files and directories. You will see there are some new directories and files.
  Lets go to each one by one

## app/

  In this directory you will see new <b>concerns/</b> dir under controllers/ and models/.
  Concerns are added to manage your models/controllers in a better way . You can

  Make your models ligh-weighted.
  Cross sharing of modules code.

  Mixins and modules code can be put under this dir.
  Lets say in our blog application we have user model and post model and we require to add comments for both model
  so we created a commentable module in our concerns/ dir and included in both model.

  {% codeblock lang:ruby %}
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
  end{%endcodeblock%}
  Now suppose we need to add one more module notable to our model post.
  {% codeblock lang:ruby %}
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
  end{%endcodeblock%}
Now you can include Commentable to Noteable
{% codeblock lang:ruby %}
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
end{%endcodeblock%}
Now both Commentabel and Notebale are accessible for Post class.
</p>

<li class='title'> bin/</li>
App executable bundle, rails and rake are under this dir. So dont need to run 'bundle exec' before any rails command.
<a href='https://github.com/sstephenson/rbenv/wiki/Understanding-binstubs'>Binstubs</a> can also be generated using bundle command ,bundle 1.3 add spports for generating binstubs.
What Binstubs are executables required to prepare the environment original executable. 
<br/>
When we do 
{% codeblock lang:ruby %}gem install cucumber{% endcodeblock %}
an executable file is genearted in gems//cucumber-1.3.2/bin dir(i am using rvm here)
{% codeblock lang:ruby %}~/.rvm/gems/ruby-1.9.3-p448/gems/cucumber-1.3.2/bin{% endcodeblock %}
<p>
Now we can generate using command
{% codeblock lang:ruby %} bundle binstubs cucumber{% endcodeblock%}
It wil genrate executable in bin/. Having executables within your app ensures they are using app ruby and gems.
</p>

<li class='title'>initializers/filter_parameter_logging.rb</li>
<p>
Now we can specify all filter parameters in this files instead of config/application.rb
Lets say we have a attribute customer_id of customer model and we want to filter in our log files then just write
</p>
{% codeblock lang:ruby %}
Rails.application.config.filter_parameters += [:customer_id]
{% endcodeblock %}
<p>
After restarting your server if there is any request having customer_id parameter. 
it will filtered the value of customer_id.
<br/>
{%img http://farm6.staticflickr.com/5347/9236715843_335affed17_b.jpg%}
</p>

<li class='title' > doc/</li>
Doc has been removed from rails directory. 
Its is extracted in "sdoc" gem and is added to Gemfile by default so still you can generate docs by running following rake task

{% codeblock lang:ruby %}
bundle exec rake doc:rails
{% endcodeblock%}

<li class='title'> script/</li> Its removed and rails executable is moved to bin/ dir


<li class='title'> test/</li>
This directory has been changed differently. 
<ul>
<li>Now we have controller and models in place of functionals and units</li>

<li>We have helpers and mailers dir under test</li>

<li>Performance test has been extracted to a new <a href='https://github.com/rails/rails-perftest'>Gem</a>. So performance dir is not under test by default
</li>
</ul>
Corresponding rake task for test are also changed -:
{% codeblock lang:ruby %}
rake test:controllers
rake test:models
rake test:helpers
rake test:mailers
{% endcodeblock %}

<li class='title'>vendor/</li> Rails plugins are already gone away so this dir is no more needed.
<li>scaffold</li>
<p>Our favorite rails command 'scaffold' gives us more new.
Lets create new scaffold
{% codeblock lang:ruby %}
rails generate scaffold list title:string 
{% endcodeblock %}
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
