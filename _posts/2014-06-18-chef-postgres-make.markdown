---
layout: post
title: "Chef - Installing Postgres - No Make file"
date: 2014-06-19 13:37:00
categories: RailsDeploy
---

I had to reformat my mac because of a bad installation of virtual box.
I'm now using Vagrant 1.5.4 and VirtualBox 4.3.6
I'm just working through all the little bug nuances. Here's one of them I thought I would mention.


{% highlight bash %}
[2014-06-19T13:34:28-05:00] WARN: Failed to properly build pg gem. Forcing properly linking and retrying (omnibus fix)
  * execute[generate pg gem Makefile] action run
    - execute /opt/chef/embedded/bin/ruby extconf.rb

  * execute[make pg gem lib] action run
================================================================================
Error executing action `run` on resource 'execute[make pg gem lib]'
================================================================================


Errno::ENOENT
-------------
No such file or directory - make


Cookbook Trace:
---------------
/var/chef/cache/cookbooks/postgresql/recipes/ruby.rb:112:in `rescue in rescue in from_file'
/var/chef/cache/cookbooks/postgresql/recipes/ruby.rb:63:in `rescue in from_file'
/var/chef/cache/cookbooks/postgresql/recipes/ruby.rb:24:in `from_file'
/var/chef/cache/cookbooks/slice/recipes/default.rb:44:in `from_file'


Resource Declaration:
---------------------
# In /var/chef/cache/cookbooks/postgresql/recipes/ruby.rb

107:     lib_maker = execute 'make pg gem lib' do
108:       command 'make'
109:       cwd ext_dir
110:       action :nothing
111:     end
112:     lib_maker.run_action(:run)



Compiled Resource:
------------------
# Declared in /var/chef/cache/cookbooks/postgresql/recipes/ruby.rb:107:in `rescue in rescue in from_file'

execute("make pg gem lib") do
  action [:nothing]
  retries 0
  retry_delay 2
  command "make"
  backup 5
  cwd "/opt/chef/embedded/lib/ruby/gems/1.9.1/gems/pg-0.17.1/ext"
  returns 0
  cookbook_name "postgresql"
  recipe_name "ruby"
end




================================================================================
Recipe Compile Error in /var/chef/cache/cookbooks/slice/recipes/default.rb
================================================================================


Errno::ENOENT
-------------
execute[make pg gem lib] (postgresql::ruby line 107) had an error: Errno::ENOENT: No such file or directory - make


Cookbook Trace:
---------------
  /var/chef/cache/cookbooks/postgresql/recipes/ruby.rb:112:in `rescue in rescue in from_file'
  /var/chef/cache/cookbooks/postgresql/recipes/ruby.rb:63:in `rescue in from_file'
  /var/chef/cache/cookbooks/postgresql/recipes/ruby.rb:24:in `from_file'
  /var/chef/cache/cookbooks/slice/recipes/default.rb:44:in `from_file'


Relevant File Content:
----------------------
/var/chef/cache/cookbooks/postgresql/recipes/ruby.rb:

105:      lib_builder.run_action(:run)
106:  
107:      lib_maker = execute 'make pg gem lib' do
108:        command 'make'
109:        cwd ext_dir
110:        action :nothing
111:      end
112>>     lib_maker.run_action(:run)
113:  
114:      lib_installer = execute 'install pg gem lib' do
115:        command 'make install'
116:        cwd ext_dir
117:        action :nothing
118:      end
119:      lib_installer.run_action(:run)
120:  
121:      spec_installer = execute 'install pg spec' do



[2014-06-19T13:34:35-05:00] ERROR: Running exception handlers
[2014-06-19T13:34:35-05:00] ERROR: Exception handlers complete
[2014-06-19T13:34:35-05:00] FATAL: Stacktrace dumped to /var/chef/cache/chef-stacktrace.out
Chef Client failed. 2 resources updated
[2014-06-19T13:34:36-05:00] FATAL: Chef::Exceptions::ChildConvergeError: Chef run process exited unsuccessfully (exit code 1)
{% endhighlight %}


First thing I found was that the make library is part of build-essentials. So if I include that with


{% highlight bash %}
include_recipe "build-essential"
{% endhighlight %}
