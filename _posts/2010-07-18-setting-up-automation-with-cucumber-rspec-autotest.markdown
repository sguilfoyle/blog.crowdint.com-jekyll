---
layout: post
title: Setting up Automation with Cucumber, RSpec, Autotest in RoR 2.3.8
author: Roberto Cataneo
email: roberto.cataneo@crowdint.com
avatar: 56839e737258faa963847dd7269a8059
---

Nowadays, one of the best practices to develop your web applications involves Behavior Driven Development or BDD, as it is also known. 

Specifically talking about Ruby on Rails, the best tool to accomplish this is definitively Cucumber for describing your features and RSpec for your unit tests, and I'd like to talk a little more, about how to set them up, so you'll be able to start using them as the very handy tool they are.

## Installing RSpec, RSpec-Rails, Cucumber, Cucumber-Rails

First of all, you need to install the cucumber and RSpec gems, (you may need to add a sudo prefix to your commands according to your system), so open up a shell and type:

{% highlight bash %}
gem install rspec-rails
{% endhighlight %}

You can type _spec --help_ to see the options available to use with rspec. Now, every time you want to run your rspec tests run the command:

{% highlight bash %}
spec spec/
{% endhighlight %}

It's time to install cucumber gem like this:

{% highlight bash %}
gem install cucumber-rails
{% endhighlight %}

It may ask you for gherkin gem in order to install cucumber, so don't forget to install it too:

{% highlight bash %}
gem install gherkin
{% endhighlight %}

After that, try to install cucumber gem again, and if there is no problem, you may see cucumber help:

{% highlight bash %}
cucumber --help
{% endhighlight %}

In order to run your cucumber scenarios, you use this command:

{% highlight bash %}
cucumber features/
{% endhighlight %}

Just as a note here, when you write your features and describe your steps for a web application it is a handy to make use of the webrat gem which includes some useful steps:

{% highlight bash %}
gem install webrat
{% endhighlight %}

## Installing ZenTest, Autotest-Rails y Autotest-Growl
 
At this point we are ready to start writing our features, but before we do that, we want to go a little bit further and install autotest, which will allow us to write our features and rspec scenarios while autotest will handle to run tests every time it detects a change in our code, really cool eh?

We need to include gemcutter as a repository in case we don't have it already:

{% highlight bash %}
gem sources -add http://gemcutter.org
{% endhighlight %}

After that we go for the Gems we need for autotest:

{% highlight bash %}
gem install autotest-rails
{% endhighlight %}

You may need to install before ZenTest, in case autotest didn't do it previously:

{% highlight bash %}
gem install ZenTest
{% endhighlight %}

And for MacOS users we can even "beautify" it a more using Growl to show us notifications about the status of our tests. So let's keep going.

In order to do that, now you have to install the autotest/growl gem:

{% highlight bash %}
gem install autotest-growl
{% endhighlight %}

Now create a  file ~/.autotest where you need to require it:

{% highlight ruby %} 
require 'autotest/growl'
{% endhighlight %}

Once you have written  your features, and rspec tests, you can use autospec for rspec and it will be waiting for you to make changes and rerun automatically your tests every time, you just have to use this command:

{% highlight bash %}
autospec
{% endhighlight %}

And you can do so to include cucumber scenarios in you autotest:

{% highlight bash %}
AUTOFEATURE=true autospec
{% endhighlight %}

You'll be getting a nice Growl notification every time you change your code and you can be aware of the status of your tests at any time.

In the case you include cucumber, it may be running the tests indefinitely which can be  a little bit annoying, also could be a memory hazard, so I encourage you to add a new file .autotest in your application root directory and write this:

{% highlight ruby %}
Autotest.add_hook(:initialize) do |at|
    at.add_exception %r{^\.git}  # ignore Version Control System
     at.add_exception %r{^./tmp}  # ignore temp files, lest autotest will run again, and again...
     at.add_exception "rerun.txt"
     at.add_mapping(%r{^lib/.*\.rb$}) do |f, _|
          Dir['spec/**/*.rb']
     end
     nil
end
{% endhighlight %}

After this you are now good to go, and you can start using cucumber and rspec to "bddify" your app development!!
