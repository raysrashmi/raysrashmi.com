---
title: Background Processing Using Rails Runner
date: 3/05/2012
tags: RailsRunner,Backgroundtask,Ruby,RubyOnRails
---

<p>Rails comes with a built-in tool for running tasks independent of the web cycle. The <code>rails runner</code> command simply loads the default Rails environment and then executes some specified Ruby code.</p>

READMORE

Some of use cases :
<ol>
  <li>Data import</li>
  <li>Execute methods of your models.</li>
  <li>Sending batch emails</li>
  <li>Nightly report generation</li>
</ol>

<p>Showing here some code example for</p>

<p>Let's say you want to generate a nightly report and send it out to multiple users</p>

<pre class="brush:ruby">
rails runner DailyReport.send_me!
</pre>

<p>This will execute <code> send_me! </code> method for <code> DailyReport </code> class.</p>

<p><code> rails runner </code> will give us access to all rails environment so we can even use the Active Record finder methods to extract data from our application.</p>

<pre class="brush:ruby">
rails runner 'User.all.map(&:email).each {|e| puts e }'
</pre>

<p>Here we are just listing out emails of all users in our system.</p>

<pre class="brush:ruby">
rails runner -h
</pre>

<p>Output will be</p>

<pre class="brush:ruby">
Usage: runner [options] ('Some.ruby(code)' or a filename)

    -e, --environment=name           Specifies the environment for the runner to operate under (test/development/production).
                                     Default: development

    -h, --help                       Show this help message.

You can also use runner as a shebang line for your scripts like this:
-------------------------------------------------------------
#!/usr/bin/env /Users/raysrashmi/checkouts/foobar/script/rails runner

Product.find(:all).each { |p| p.price *= 2 ; p.save! }
-------------------------------------------------------------
</pre>

A sample crontab to run that script might look like

<pre class="brush:ruby">
  @daily  /Users/raysrashmi/checkouts/foobar/script/rails runner -e production 'DailyReport.send_me!'
</pre>

<p>This script will run daily to send out daily report.

The Rails Runner is useful for short tasks that need to run infrequently.

But jobs that require more heavy work are best handled by other libraries.

I use <code> rails runner </code> whenever I need to run a small task.
</p>
Reference links :
<ul>
  <li><a href="http://www.ameravant.com/posts/recurring-tasks-in-ruby-on-rails-using-runner-and-cron-jobs">http://www.ameravant.com/posts/recurring-tasks-in-ruby-on-rails-using-runner-and-cron-jobs</a></li>
</ul>


There are other better library available for handling background process. But here I wanted to talk about <code>rails runner</code>