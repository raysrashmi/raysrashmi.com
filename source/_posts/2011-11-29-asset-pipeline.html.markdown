---
title: Asset Pipeline with Rails 3.1
date: 29/11/2011
tags: Asset Pipeline,Rails3
---

<p>
This is the new very interesting and useful  feature in Rails 3.1.
</p>

<strong>Asset Pipeline</strong>
<ul>
    <li>The asset pipeline provides a framework to concatenate and minify or compress JavaScript and CSS assets.</li>
    <li>It allows you to write these assets in other languages like CoffeeScript, Sass and ERB</li>
    <li>Asset pipeline helpful in production environment cause it will reduce the number of request which browser make.</li>
</ul>
<p>Prior to Rails 3.1 these features were added through third-party Ruby libraries such as Jammit and Sprockets. Rails 3.1 is integrated with Sprockets through Action Pack which depends on the sprockets gem, by default.</p>

<strong>Asset Organization</strong>
<div>Prior to Rails 3.1 -:</div>
<ul>
	<li>Currently we have all assets in one directory public.we are putting all javascripts either it is third party javascirpt or your own javascript.Its not good way to treat them because like in rails we have a very good organization to put our code file.</li>
	<li>Its not not easy to update our javascirpt code(like if we are using third party code)</li>
</ul>
In Rails 3.1-:

There are three directories
<ul>
	<li><strong>app/assets-:</strong> it holds the code write for specific appication</li>
	<li><strong>lib/assets -:</strong> it holds the generic code we write for our application(for example if we extending any jQuery function)</li>
	<li><strong>vendor/assets -:</strong> it holds the third party code(like we are using any jQuery plugin)</li>
</ul>
<p>so if you want to access application.css in app/assets/stylesheets.you can just access the url localhost:3000/assets/application.css</p>
<p>
you can add additional path to the pipeline in config/application.rb. For example:
<pre class="brush:ruby">config.assets.paths &lt;&lt; Rails.root.join("app", "assets", "flash")</pre>
</p>
<strong>Manifest files and Directives</strong>
<ul>
	<li>Manifest files used by Sprockets(a gem support Asset pipe line its is added in rails 3.1 app by default),in these files contains instruction that tell Sprockets which files are required to build a single CSS or Javascript file,these instructions are called Directives</li>
	<li>In Rails 3.1 Default Directives are application.js in app/assets./javascripts directory and application.css in app/assets/stylesheets</li>
</ul>
<strong>Assets In Development </strong>
<p>
In Development environment all assets are served as individual files
For Example in app/assets/javascripts/application.js
</p>

    //= require core
    //= require blogs</blockquote>


after converting in HTML it will look like

    <script src="/assets/core.js?body=1" type="text/javascript"></script>
    <script src="/assets/blogs.js?body=1" type="text/javascript"></script>


<p>If turn off debugging by setting config.assets.debug = false in config/environments/development.rb
above file will generate</p>
    <script src="/assets/application.js?body=1" type="text/javascript"></script>

<strong>In Production</strong>
<p>
Rails server complied assets using <strong>Fingerprinting Scheme</strong>
</p>
<p>
<strong>In Fingerprinting</strong> scheme a hash of  content is attached to the end of file to uniquely identified the file
blogs-908e25f4bf641868d8683022a5b62f54.css
</p>
<p>
Prior rails 3.1 a time stamp query string is attached to file name
<span>stylesheets/blogs.css?1409495796</span>
</p>

<p>For example -:</p>
    <%= javascript_include_tag :application%>
    <%= stylesheet_link_tag "application" %>

It will compile in to

    <script src="/assets/application-908e25f4bf641868d8683022a5b62f54.js" type ="text/javascript"></script>
    <link href="/assets/application-4dd5b109ee3439da54f5bdfd78a80473.css" media="screen" rel="stylesheet" type="text/css">

<p>
The fingerprinting behavior is controlled by the setting of <b>config.assets.digest</b> setting in Rails (which is <b>true</b> for production, <b>false</b> for everything else)
</p>
<strong>Useful Links -:</strong>

<ul>
<li>
  <a href="http://railscasts.com/episodes/279-understanding-the-asset-pipeline" target="_blank">http://railscasts.com/episodes/279-understanding-the-asset-pipeline</a>
</li>
<li><a href="http://guides.rubyonrails.org/asset_pipeline.html" target="_blank">http://guides.rubyonrails.org/asset_pipeline.html</a></li>
</ul>
