---
layout: post
title: OAuth with OmniAuth and Github
date: 22/04/2012
tags: Github,OAuth,OmniAuth
---


<p>
If you want to authenticate your user in your app with Github  use omniauth-github strategy for that.
Here I am describing how you can do that very quickly.

OmniAuth provides list of  strategies to use many OAuth for your application. Here is the 
<a href="https://github.com/intridea/omniauth/wiki/List-of-Strategies">List of Strategies</a>.
</p>

<!--more-->

<p>
  Github uses the OAuth2 flow.

  First of all you have to register your app with Github <a href="https://github.com/settings/applications">GitHub Applications Page</a>.
  Create a new application here and get Client ID and Secret.

  Here is how can you obtain Client ID and Secret from Github.
</p>

<p>
  {%img http://farm4.staticflickr.com/3716/9240891751_409f794720_m.jpg%}
</p>

<p>
Steps to use these in your app.
{% codeblock lang:ruby%} rails new GithubAuthApp{% endcodeblock %} 
Update your Gemfile add <code> omniauth-github </code>
{% codeblock lang:ruby%} gem "omniauth-github"{% endcodeblock %} 
Create a config/initializers/omniauth.rb file.
Paste your key instead of XXXX, and secret instead of YYYY
{% codeblock lang:ruby%} 
use OmniAuth::Builder do
  provider :github, XXXX, YYYY
end
{% endcodeblock %} 
</p>

<p>
All done start you server</p>
{% codeblock lang:ruby%} bundle exec rails server{% endcodeblock %} 
Just open your app in browser with URL
{% codeblock lang:ruby%} http://localhost:3000/auth/github{% endcodeblock %} 
You will be redirecting to Github for authentication.

After success your app will redirect to your given callback URL with information and token!

<p>At <a href="http://www.omniauth.org/">OmniAuth.org</a> you can try out different -2 Strategies.</p>

Useful links :
<ul>
	<li><a href="http://www.omniauth.org/">OmniAuth.org</a> One page for all you need.</li>
	<li><a href="https://github.com/intridea/omniauth-github">omniauth-github</a> gem.</li>
</ul>
