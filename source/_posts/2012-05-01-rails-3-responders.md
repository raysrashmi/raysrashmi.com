---
title: Rails 3 Responders
date: 1/05/2012
tags: Rails3,RailsResponders
---

<p>So we have a problem here in Controllers.

As our application grows our controllers grows. We will be in trouble when we have alternative formats in our controllers like JSON, XML.
</p>

You might need to respond both in JSON and XML or might be only JSON in your controller.
<pre class="brush:ruby">
class BlogsController &lt; ApplicationController
   def index
     @blogs = Blog.all
     respond_to do |format|
     format.html
     format.xml { render :xml =&gt; @blogs }
    end
  end

  def show
    @blog = Blog.find(params[:id])
    respond_to do |format|
      format.html
      format.xml { render :xml =&gt; @blog }
    end
  end
end
</pre>
<p>You can see here that in both the action we are responding with HTML and XML.

Rails 3 introduced a new set of methods called <code>responders</code> that abstract the code so that the controller becomes much simpler.
</p>

Here is our example written using <code>responders</code>
<pre class="brush:ruby">
  class BlogsController &lt; ApplicationController
  respond_to :html, :xml

  def index
    @blogs = Blog.all
    respond_with(@blogs)
  end

  def show
    @blog = Blog.find(params[:id])
    respond_with(@blog)
  end
end</pre>
<p>This looks more cleaner No ?

The entire <code>respond_to</code> block is gone now.
</p>

<p></p><strong>I know this feature is very old. But it's very important to use this feature for more cleaner code.</strong></p>

Reference links :
<ul>
  <li><a href="http://archives.ryandaigle.com/articles/2009/8/6/what-s-new-in-edge-rails-cleaner-restful-controllers-w-respond_with">http://archives.ryandaigle.com/articles/2009/8/6/what-s-new-in-edge-rails-cleaner-restful-controllers-w-respond_with</a></li>
  <li><a href="http://blog.plataformatec.com.br/tag/respond_with/">http://blog.plataformatec.com.br/tag/respond_with/</a></li>
</ul>


