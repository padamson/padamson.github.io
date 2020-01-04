---
layout: page
title: Reminders 
permalink: /reminders/
---

# [Making use of external R code in knitr and R markdown](http://zevross.com/blog/2014/07/09/making-use-of-external-r-code-in-knitr-and-r-markdown/)

# [The Perfect Workflow, with Git, GitHub, and SSH](https://code.tutsplus.com/tutorials/the-perfect-workflow-with-git-github-and-ssh--net-19564)

{% comment %}
	{% include links-ad.html %}
{% endcomment %}

# Dreaded `RVM is not a function...` 

Credit: [mpapis](http://stackoverflow.com/users/497756/mpapis)'s 
[answer on StackExchange](http://stackoverflow.com/a/14289460/4257137)

Make sure your shell initialization files are set up properly:

{% highlight bash %}
rvm get head --auto-dotfiles
{% endhighlight %}

Then go to your terminal emulator preferences and enable login shell, 
Sometimes it it required to use `/bin/bash --login`. Also make sure to fully 
close terminal and open it fresh after changing the setting.

# Create New Rails App with Project-Specific Gemset

Credit: [Daniel Kehoe's Post on RailsApps Project](http://railsapps.github.io/installrubyonrails-mac.html)

{% highlight bash %}
 mkdir myapp
 cd myapp
 rvm use ruby-2.3.1@myapp --ruby-version --create 
 gem install rails
 rails new .
{% endhighlight %}

Replace `myapp` appropriately. 

The RVM command creates a new project-specific gemset, the option 
`—ruby-version` creates `.ruby-version` and `.ruby-gemset` files in the root 
directory. RVM recognizes these files in an app’s root directory and loads the 
required version of Ruby and the correct gemset whenever you enter the 
directory.

Since the newly created gemset is empty (except for all the gems in the 
global gemset), we immediately install Rails. The command `gem install rails` 
installs the most recent release of Rails. If you want a specific version
of Rails, use `gem install rails --version=3.2.18` or whatever.

Finally we run `rails new .` to assign the name of the directory to the 
new application and actually create it. Obviously, you can adapt this command
to take advantage of other Rails capabilities. For example, if you wish, you 
can use the Rails Composer tool to generate a starter application with a 
choice of basic features and popular gems.
