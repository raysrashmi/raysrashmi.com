---
layout: post
title: Routes in Rails3
date: 4/12/2010
tags: Routes in Rails3,Routes,Rails
---

<h1>Rails3 define routes in more efficient way</h1>

Routes for CRUD actions
{% codeblock lang:ruby%}
     resources   :users     
{% endcodeblock %}

Also define for multiple resources
{% codeblock lang:ruby%}
     resources :users,  :blogs, :books     
{% endcodeblock %}

Nested Routes

     
{% codeblock lang:ruby%} 
  resources :users do
    resources  :blogs
  end
{% endcodeblock %}
     

Member and collection

{% codeblock lang:ruby%} 
  resources :blogs do
    :get => :preview, :on =>:member
    :get => :list, :on =>:collection
  end
{% endcodeblock %}
     


if  we have more than one collection or member functions

     
{% codeblock lang:ruby%} 
resources  :blogs do
    member do
     :get  :preview
     :put :sort
    end
   collection do
     :get :list
     :get :detailed_info
   end
end
{% endcodeblock %}
     



Named Routes

{% codeblock lang:ruby%} 
  match "history" => "site#index", :as => :history
{% endcodeblock %}

Adding :as makes it a named route so that we can use history_path      or      history_url in our application.

Route for Root
{% codeblock lang:ruby%}
     
  root   :to => "home#index"
     
{% endcodeblock %}

Constraints and Parameters in routes
{% codeblock lang:ruby%}

     
  match "search/:email(/:first_name/:last_name)" => "users#search",  :constraints => {:email => /\A([^@\s]+)@((?:[-a-z0-9]+\.)+[a-z]{2,})\Z/i}
     

{% endcodeblock %}
Here email is mandatory parameter and first_name and last is optional parameter and in constraints email format is defined
