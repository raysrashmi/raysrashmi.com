---
title: jQuery Date/Time picker
date: 15/05/2011
tags: date/time picker,jQuery
---

To handle date and time both in calender use jQuery date time picker add-on

you need to inlcude javascript files
<pre class="brush:ruby">
jquery-1.4.1.min
jquery-ui-1.8.9.custom.min
jquery-ui-timepicker-addon
</pre>

Inclue CSS files
<pre class="brush:ruby">jquery-ui-1.8.9.custom
</pre>

<pre class="brush:ruby">
<%#= text_field_tag ' name','',:class => 'date_value'%>
jQuery('.'date_value).datetimepicker({
  dateFormat: 'MM dd,yy'
});
</pre>
