---
title: Basic Difference between Proc and Lambda in Ruby
date: 19/02/2012
tags: Proc,Lambda,Ruby
---

<strong>Proc</strong> =&gt; <tt><a href="http://www.ruby-doc.org/core-1.9.3/Proc.html">Proc</a></tt> objects are blocks of code that have been bound to a set of local variables. Once bound, the code may be called in different contexts and still access those variables.

<pre class="brush:ruby">
my_proc = proc do |a|
  puts "number is #{a}"
end
my_proc.call(10 ) # number is 10
</pre>
<strong>lambda</strong>
<pre class="brush:ruby">
my_lambda = lambda do|a|
  puts "number is #{a}"
end
my_lambda.call(10 ) # number is 10
</pre>
<strong>Difference between both of them</strong>in Ruby-1.9.3
<ol>
  <li>
    lambda is very specific for parameters passing to it but proc not -:
    <pre class="brush:ruby">
    my_proc = proc do|a|
      puts "this is proc"
    end
    my_proc.call #this is proc
    </pre>
    <pre class="brush:ruby">
    my_lambda =lambda do |a|
      puts "this is lambda"
    end
    my_lambda.call #will throw error "wrong number of arguments"
    </pre>

    In ruby-1.8.7 it just give you warning not error
  </li>
  <li>
    Second is about return mean -:
    <pre class="brush:ruby">
    def run_a_proc(p)
      puts "starting proc"
      p.call
      puts "Ending proc"
    end

    def test_proc
      run_a_proc lambda{puts 'this is lambda'; return}
      run_a_proc proc{puts 'this is proc'; return}
    end
    test_proc
    </pre>
    the output is -:

    <pre class="brush:ruby">
    starting proc
    this is lambda
    Ending proc
    starting proc
    this is proc
    </pre>

  </li>
</ol>

<p>
so difference is lambda return within it but the proc return from where it is called

if we modified test_proc method like
</p>
<pre class="brush:ruby">
def test_proc
  run_a_proc proc{puts 'this is proc'; return}
  run_a_proc lambda{puts 'this is lambda'; return}
end
</pre>
now the output will be -:

<pre class="brush:ruby">
starting proc
this is proc
</pre>

