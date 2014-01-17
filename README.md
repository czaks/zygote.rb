zygote.rb
=========

"Snapshot" ruby application for easy and fast restart


About
-----

This is a small ruby script, that takes advantage of operating system Copy-On-Write fork behaviour. It should optimize for some usecases, eg. when you develop a large application and require a great amount of rubygems.


Usage
-----

```ruby
# Here we do some high computation on application start
require "applogic"
do_some_ruby_logic_that_takes_much_time

# This snapshots our application, so that after quit it can be relaunched.
require "zygote"
Zygote.spawn

# The rest of your app logic
puts "My example app has been successfully launched!"
require "my_module"
MyModule.probably_faulty_code
```

```
$ ./app.rb
Initializing application... (may take a long time) done!
       # this is where zygote snapshots a running process
My example app has been successfully launched!
Doing my probably faulty code...
       # here you stop ruby by ctrl+c or an exception happens. in the meantime, we modify my_module
Continue (n to stop)? y
       # zygote restarts the application; note that we don't have to wait for the startup initialization
My example app has been successfully launched!
Doing my probably fixed code...
(...)

```

Platforms
---------

It should work on all Unix-type platforms, where fork(2) syscall is available.
