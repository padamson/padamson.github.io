---
layout: page
title: Reminders 
permalink: /reminders/
---

{% include links-ad.html %}

# Dreaded `RVM is not a function...` 

Thanks to [mpapis](http://stackoverflow.com/users/497756/mpapis)'s 
[answer on StackExchange](http://stackoverflow.com/a/14289460/4257137). 
Make sure your shell initialization files are set up properly:

{% highlight bash %}
rvm get head --auto-dotfiles
{% endhighlight %}

Then go to your terminal emulator preferences and enable login shell, 
Sometimes it it required to use `/bin/bash --login`. Also make sure to fully 
close terminal and open it fresh after changing the setting.

