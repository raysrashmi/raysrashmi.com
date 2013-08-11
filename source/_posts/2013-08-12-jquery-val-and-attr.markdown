---
layout: post
title: "jQuery attr('value') and val()"
date: 2013-08-12 00:11
comments: true
categories: jquery
tags: jquery, val(), attr('value')
---
Here is a short tip about using **$(obj).val()** and **$(obj).attr('value')**.
I was fixing a bug in ajax request with some params
then i found that there is a difference <code>val()</code> method and <code>attr('value')</code>
I never faced this problem before. I thought to share this with you guys.

<!--more-->

I had an text field

{% codeblock lang:html%}
<input class="blur_text" id="search_box_location_input" name="location" placeholder="Address, City, State or Zip" type="text" value="New York">
{% endcodeblock %}

I had to send the value of this text field to server side

So if I do

{% codeblock lang:html%}
  $("#search_box_location_input").attr('value') => "New York"
{% endcodeblock %}

Result is "New York"

{% codeblock lang:html%}
  $("#search_box_location_input").val() => "Address, City, State or Zip"
{% endcodeblock %}
It gives the the value entered in text field and if nothing then the placeholder value

It means <code>val()</code> gives the actual value that is there in text field. <code>$(obj).attre('value')</code> it actually give the value of attribute 'value'


Its is better to use <code>**val()**</code> method to get the value of input field

Read about <code>val()</code> method and <code>attr</code>

  * http://api.jquery.com/val/
  * http://api.jquery.com/attr/


