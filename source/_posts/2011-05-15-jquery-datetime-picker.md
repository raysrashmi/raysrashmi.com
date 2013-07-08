---
layout: post
title: jQuery Date/Time picker
date: 15/05/2011
tags: date/time picker,jQuery
---

To handle date and time both in calender use jQuery date time picker add-on

you need to inlcude javascript files
{% codeblock lang:ruby%}
jquery-1.4.1.min
jquery-ui-1.8.9.custom.min
jquery-ui-timepicker-addon
{% endcodeblock %}

Inclue CSS files
{% codeblock lang:ruby%}jquery-ui-1.8.9.custom
{% endcodeblock %}

{% codeblock lang:ruby%}
<%#= text_field_tag ' name','',:class => 'date_value'%>
jQuery('.'date_value).datetimepicker({
  dateFormat: 'MM dd,yy'
});
{% endcodeblock %}
