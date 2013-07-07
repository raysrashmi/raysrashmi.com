---
title: Twitter Client script to retrieves all the unique http links in the last recent tweets
date: 31/12/2010
tags: Twitter,Client Script
---

# Code to fetch link from tweets matched by given hash_tag


<blockquote>
<pre>
require 'rubygems'
require 'twitter'


hash_tag = ARGV.first

if hash_tag.nil?
  puts "Please provide tag you want to fetch. Example 'rails', '#rails'"
  exit
end
puts 'fetching......'
begin
  tweets = Twitter::Search.new.hashtag(hash_tag)
rescue Exception => e
  # Time out or any other error can occur here
  puts "Errors in fetching tweets #{e}"
end
links = []
tweets.each do |tweet|
  links << tweet.text.match(/\bhttps?:\/\/\S+\b/)
end

links = links.uniq
puts links
</pre>
</blockquote>

