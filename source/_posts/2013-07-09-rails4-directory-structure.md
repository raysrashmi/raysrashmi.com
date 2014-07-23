---
layout: post
title: Rails 4 Directory Structure
date: 09/07/2013
tags: rails4, rails
keywords: rails4, rails
description: rails 4 directory structure
---

{%img http://farm6.staticflickr.com/5443/9241313505_ac035bd643_m_d.jpg%}

Rails 4 is released. Lots of new features has been added to rails.
Here we will see what new directories have been added.

  We will create a sample app in rails 4 so that it will be easy to see the new structure.
  I am assuming  you have updated your rails.
  To check current version of rails run following command:
  {% codeblock lang:ruby %}rails -v{%endcodeblock%}
  It should be Rails 4.0.0

<!--more-->

  Now create a very simple rails application
  {% codeblock lang:ruby %}rails new  blog  {% endcodeblock %}

  It creates a our app directory . You will see there are some new directories and files.
  Lets go to each one by one

## app/

  In this directory there is new <b>concerns/</b> dir under controllers/ and models/.
  <a href='http://37signals.com/svn/posts/3372-put-chubby-models-on-a-diet-with-concerns'>Concerns</a>
  are added to manage your models/controllers in a better way .

  Mixins and modules code can be put under this dir.
  Lets say in our blog application we have user model and post model and we require to add comments for both model
  so we created a commentable module in our concerns/ dir and included in both model.

  {% codeblock lang:ruby %}
  module Commentable
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
  Now suppose we need to add one more module noteable to our model post.
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
But we can also include Commentable to Noteable.
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
Because of line "extend ActiveSupport::Concern
" both Commentable and Noteable are accessible for Post class.

Its easy to cross sharing of code among models and to make your model clean.

## bin/
App executables bundle, rails and rake are under this dir. So dont need to run 'bundle exec' before any rails command.
<a href='https://github.com/sstephenson/rbenv/wiki/Understanding-binstubs'>Binstubs</a> can also be generated using bundle command ,bundle 1.3 added spports for generating binstubs.
What Binstubs are executables required to prepare the environment before original executable.

When we do
{% codeblock lang:ruby %}gem install cucumber{% endcodeblock %}
an executable file is genearted in gems//cucumber-1.3.2/bin dir(i am using rvm here)
{% codeblock lang:ruby %}~/.rvm/gems/ruby-1.9.3-p448/gems/cucumber-1.3.2/bin{% endcodeblock %}

Now we can generate using bundle command
{% codeblock lang:ruby %} bundle binstubs cucumber{% endcodeblock%}
It wil genrate executable in bin/. Having executables within your app ensures that they are using app ruby and gems.

## initializers/filter_parameter_logging.rb

We use config.filter_parameters in config/application.rb to filter out parameters which we dont want to show in log files.
But Its moved to filter_parameter_logging.rb under initializers dir

Lets say we have an attribute customer_id of customer model and we want to filter in our log files then just write
{% codeblock lang:ruby %}
Rails.application.config.filter_parameters += [:customer_id]
{% endcodeblock %}

After restarting your server if there is any request having customer_id parameter.
it will not show the value of customer_id
{%img http://farm6.staticflickr.com/5347/9236715843_335affed17_b.jpg%}

##doc/

Doc has been removed from rails directory.
Its is extracted in <a href='https://github.com/voloko/sdoc/'>sdoc</a> gem and added to Gemfile by default so still you can generate docs by running following rake task

{% codeblock lang:ruby %}
bundle exec rake doc:rails
{% endcodeblock%}

##script/
It has been removed and rails executable is moved to bin/ dir


##test/
This directory has been changed quite a bit.

 * Now we have controllers and models in place of functionals and units</li>

 * We also have helpers and mailers dir under test by default</li>

Performance test has been extracted to a new <a href='https://github.com/rails/rails-perftest'>Gem</a>. So performance dir is not under test by default

Corresponding rake task for test has also changed -:
{% codeblock lang:ruby %}
rake test:controllers
rake test:models
rake test:helpers
rake test:mailers
{% endcodeblock %}

##vendor/
Rails plugins are already gone away so this dir is no more needed.

##scaffold
   Our favorite rails command 'scaffold' gives us few new files.
Lets create new scaffold
{% codeblock lang:ruby %}
rails generate scaffold list title:string
{% endcodeblock %}
You will see there are two new files

  * index.json.builder
  * show.json.builder

These files can be used for responding data in JSON format.
These are generated by <a href='https://github.com/rails/jbuilder'>jbuilder</a>
<a href='http://rubygems.org/'>Gem</a>. It is included in Gemfile by default.

###Wrap up

I hope this post will be useful for you. If there is any mistake in this post
just let me know.

There is lot to learn about rails 4. If you want here are some links

 * http://railscasts.com/episodes/400-what-s-new-in-rails-4?view=asciicast
 * http://edgeguides.rubyonrails.org/4_0_release_notes.html
