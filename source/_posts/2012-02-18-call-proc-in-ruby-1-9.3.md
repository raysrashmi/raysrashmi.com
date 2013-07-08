---
layout: post
title: Proc call in ruby1.9.3
date: 18/02/2012
tags: Proc In Ruby,Ruby
---
<p>
  Call a proc in different ways
</p>
{% codeblock lang:ruby%}
my_proc = Proc.new do|name|
  "this is #{name}"
end
{% endcodeblock %}
READMORE
You can call this proc by 4 ways
{% codeblock lang:ruby%}
my_proc.call('rays')
my_proc.('rashmi')
my_proc.("rita")
my_proc === "richa"
{% endcodeblock %}


