---
title: Polymorphic Association with has_many :through
date: 7/06/2012
tags: activerecord,association,polymorphic
---
Hi Folks,
<p>
In Rails we have Ploymorphic Association whenever we need to connect one model to more than one model.
I had a situation where i need polymorphic association with has_many :through
Here I am showing an simple example of <b>polymorphic association with <code>has_many :through</code></b>
</p>
<p>
We have <code>Contact</code> , <code>Plan</code> and <code>Template</code> as a <code>ActiveRecord</code> Classes.
</p>
<p>
We have following scenarios.
 <ol>
   <li><code>Plan has_many Contacts</code></li>
   <li><code> Contant has_many Plans </code> which is vice versa of 1.</li>
   <li><code>Template has_many Contacts</code></li>
   <li><code>Contact has_many Templates</code> which is vice versa of 3.</li>
 </ol>
</p>
<p><strong>So here is the main problem. Here to solve this we need two more tables other then <code>Plans</code> , <code>Contants</code>  and <code>Templates</code>. </strong></p>

<p>My <code> Plan </code> Model looks like:</p>

<pre class="brush:ruby">
class Plan < ActiveRecord::Base
  has_many :plan_contacts
  has_many :contacts ,:through => :plan_contacts
end
</pre>

<p>My <code> Template </code> Model looks like:</p>

<pre class="brush:ruby">
class Template < ActiveRecord::Base
  has_many :template_contacts
  has_many :contacts ,:through => :template_contacts
end
</pre>

<p>My <code> Contact </code> Model looks like:</p>

<pre class="brush:ruby">
class Contact < ActiveRecord::Base
  has_many :template_contacts
  has_many :plan_contacts

  has_many :templates ,:through => :template_contacts
  has_many :plans ,:through => :plan_contacts

end
</pre>
<p>
And here are two different tables for handling <code>many_to_many</code>
</p>
<p>
Handling <code>Plan</code> and <code> Contact</code> here.
</p>

<pre class="brush:ruby">
class PlanContact < ActiveRecord::Base
  belongs_to :plan
  belongs_to :contact
end
</pre>
<p>Handling <code>Template</code> and <code> Contact</code> here.</p>


<pre class="brush:ruby">
class TemplateContact < ActiveRecord::Base
  belongs_to :template
  belongs_to :contact
end
</pre>
<p><strong>This problem can be solved using only one table. Just we need to mark that table as a polymorphic.</strong></p>


<p>We are calling that tables as <code> contact_details</code>. Here is class looks like.</p>

<pre class="brush:ruby">
class ContactDetail < ActiveRecord::Base
  belongs_to :contactable, :polymorphic => true
  belongs_to :contact
end
</pre>

<p>Migration should look like:</p>

<pre class="brush:ruby">
class CreateContactDetails < ActiveRecord::Migration
  def change
    create_table :contact_details do |t|
      t.integer :contact_id
      t.integer :contactable_id
      t.string :contactable_type
      t.timestamps
    end
  end
end

</pre>

<p>Now change your <code> Contact </code> model like:</p>

<pre class="brush:ruby">
class Contact < ActiveRecord::Base
  has_many :plans ,:through => :contact_details, :source => :contactable, :source_type => 'Plan'
  has_many :templates ,:through => :contact_details, :source => :contactable, :source_type => 'Template'
  has_many :contact_details
  accepts_nested_attributes_for :plans,:templates
end
</pre>

<p>Now change your <code> Plan </code> model like:</p>

<pre class="brush:ruby">
  class Plan < ActiveRecord::Base
    has_many :contacts,:through => :contact_details
    has_many :contact_details, :as => :contactable
  end
</pre>

<p>Same way <code> Template </code> model like:</p>

<pre class="brush:ruby">
  class Template < ActiveRecord::Base
    has_many :contacts,:through => :contact_details
    has_many :contact_details, :as => :contactable
  end
</pre>


<p>So you can see we can use one table(contact_details) for both plan and template</p>
<pre class="brush:ruby">
 contact  = Contact.create(:name => 'Rays')
 template = Template.create(:name => 'template 1')
 plan = Plan.create(:name =>'plan')

 contact.plans << plan
 contact.templates << template

 template.contacts => [contact]
 plan.contacts => [contact]
 contact.templates => [template]
 contact.plans => [plans]
</pre>
<p>so if you have <strong>accepts_nested_attributes_for :template</strong> then you can create template for contact in a form</p>
<pre class="brush:ruby">
 Contact.create(:name => 'abc',:templates_attributes => {'0' => {'name' => 'Tech'}})
</pre>

<p> it will create an entry in templates table and and entry in contact_details</p>
<p>So in this way we dont need to write much code.we can reduce tables number </p>

<p>I hope it will be helpful for you</p>

<p><strong>Reference links :</strong>
 <ul>
   <li><a href="http://guides.rubyonrails.org/association_basics.html#the-has_many-through-association" title="http://guides.rubyonrails.org/association_basics.html#the-has_many-through-association">http://guides.rubyonrails.org/association_basics.html#the-has_many-through-association</a></li>
   <li><a href="http://railscasts.com/episodes/154-polymorphic-association" title="http://railscasts.com/episodes/154-polymorphic-association">http://railscasts.com/episodes/154-polymorphic-association</a></li>
 </ul>


</p>
