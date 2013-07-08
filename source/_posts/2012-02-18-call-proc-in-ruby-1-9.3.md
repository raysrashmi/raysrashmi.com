---
layout: post
title: Proc call in ruby1.9.3
date: 18/02/2012
tags: Proc In Ruby,Ruby
---
<p>
  Call a proc in different ways
</p>
<pre class="brush:ruby">
my_proc = Proc.new do|name|
  "this is #{name}"
end
</pre>
READMORE
You can call this proc by 4 ways
<pre class="brush:ruby">
my_proc.call('rays')
my_proc.('rashmi')
my_proc.("rita")
my_proc === "richa"
</pre>


