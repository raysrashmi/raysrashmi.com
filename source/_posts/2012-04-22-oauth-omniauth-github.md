---
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

READMORE

<p>
  Github uses the OAuth2 flow.

  First of all you have to register your app with Github <a href="https://github.com/settings/applications">GitHub Applications Page</a>. Create a new application here and get Client ID and Secret.

  Here is how can you obtain Client ID and Secret from Github.
</p>

<p>
  <a href="http://raysrashmi.com/wp-content/uploads/2012/04/github_app.jpg"><img class="aligncenter size-full wp-image-279" title="github_app" src="http://raysrashmi.com/wp-content/uploads/2012/04/github_app.jpg" alt="" width="678" height="498" /></a>
</p>

<p>
Steps to use these in your app.
<pre class="brush:ruby">rails new GithubAuthApp</pre>
Update your Gemfile add <code> omniauth-github </code>
<pre class="brush:ruby">gem "omniauth-github"</pre>
Create a config/initializers/omniauth.rb file.
Paste your key instead of XXXX, and secret instead of YYYY
<pre class="brush:ruby">
use OmniAuth::Builder do
  provider :github, XXXX, YYYY
end
</pre>
</p>

<p>
All done start you server</p>
<pre class="brush:ruby">bundle exec rails server</pre>
Just open your app in browser with URL
<pre class="brush:ruby">http://localhost:3000/auth/github</pre>
You will be redirecting to Github for authentication.

After success your app will redirect to your given callback URL with information and token!

<p>At <a href="http://www.omniauth.org/">OmniAuth.org</a> you can try out different -2 Strategies.</p>

Useful links :
<ul>
	<li><a href="http://www.omniauth.org/">OmniAuth.org</a> One page for all you need.</li>
	<li><a href="https://github.com/intridea/omniauth-github">omniauth-github</a> gem.</li>
</ul>