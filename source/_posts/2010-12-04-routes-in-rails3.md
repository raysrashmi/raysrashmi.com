---
title: Routes in Rails3
date: 4/12/2010
tags: Routes in Rails3,Routes,Rails
---

<h1>Rails3 define routes in more efficient way</h1>

Routes for CRUD actions
<blockquote>
<code>resources   :users</code>
</blockquote>

Also define for multiple resources
<blockquote>
<code>resources :users,  :blogs, :books</code>
</blockquote>

Nested Routes
<blockquote>
<code>
<pre>
  resources :users do
    resources  :blogs
  end
</pre>
</code>
</blockquote>

Member and collection
<blockquote>
<code>
<pre>
  resources :blogs do
    :get => :preview, :on =>:member
    :get => :list, :on =>:collection
  end
</pre>
</code>
</blockquote>

if  we have more than one collection or member functions
<blockquote>
<code>
<pre>
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
</pre>
</code>
</blockquote>


Named Routes


<blockquote>
<code>
<pre>
  match "history" => "site#index", :as => :history
</pre>
</code>
</blockquote>

Adding :as makes it a named route so that we can use <code>history_path</code> or <code>history</code><code>_url</code> in our application.

Route for Root
<blockquote>
<code>
  root   :to => "home#index"
</code>
</blockquote>

Constraints and Parameters in routes
<blockquote>
<code>
  match "search/:email(/:first_name/:last_name)" => "users#search",  :constraints => {:email => /\A([^@\s]+)@((?:[-a-z0-9]+\.)+[a-z]{2,})\Z/i}
</code>
</blockquote>
Here email is mandatory parameter and first_name and last is optional parameter and in constraints email format is defined