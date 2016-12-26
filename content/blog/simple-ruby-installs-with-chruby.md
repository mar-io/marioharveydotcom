+++
date = "2015-04-06T00:10:33-05:00"
title = "Simple Ruby installs with Chruby"
draft = false

+++
![cover](/content/images/2015/Mar/cherryblossoms.jpg)
Ruby version managers like rvm and rbenv are proven tools that certainly will get the job done. However, it's hard to overlook deficiencies with these tools that make them less than elegant to use at times. 

RVM can be this monolithic beast that has to take over the entire system/server. You'll quickly run into issues if you have to flip between ruby versions or have multi-user setups where you will find yourself using ugly wrappers like rvmsudo.

My experience with rbenv has been admittedly less but I find it to be somewhat confusing to set up and easy to misconfigure which can lead to headaches when all you want to do is install ruby 2.2.2 and ruby 1.8.3 and switch back and forth.

[**Chruby**](https://github.com/postmodern/chruby) (*change ruby*) and [**ruby-install**](https://github.com/postmodern/ruby-install) to the rescue!

Chruby is a tool that will manage which rubies are on your system. Ruby-install will install the versions of ruby you want.

For this to work you need to install some prerequisite packages:

On ubuntu/debian:
`$ sudo apt-get install build-essential`

On centos/rhel:
`$ yum groupinstall "Development Tools"`

The above commands will install the necessary c++ build tools and compilers needed to build chruby, ruby-install, and compile/build the downloaded rubies.

You can read more about installing chruby on the github project's [readme](https://github.com/postmodern/chruby/blob/master/README.md). Installing chruby on most linux distros is **simple**:

    $ wget -O chruby-0.3.9.tar.gz https://github.com/postmodern/chruby/archive/v0.3.9.tar.gz
    $ tar -xzvf chruby-0.3.9.tar.gz
    $ cd chruby-0.3.9/
    $ sudo ./scripts/setup.sh

After chruby is installed simply add this to .bashrc or .zshrc:

    source /usr/local/share/chruby/chruby.sh
    source /usr/local/share/chruby/auto.sh

Now source your shell rc file:
    `$ source ~/.bashrc`

Now keep in mind that when you install new rubies you simply source your shell's rc file as above.

Since we installed chruby, we can execute chruby from the command line and it will show you what rubies are available and the current ruby that is selected.

`$ chruby`

Since we have no ruby installed this command will return nothing.

Let's install **ruby-install** to install some rubies. You can read more about ruby-install at the github project's [readme](https://github.com/postmodern/ruby-install/blob/master/README.md).

Again, installing ruby-install is simple:

    $ wget -O ruby-install-0.5.0.tar.gz https://github.com/postmodern/ruby-install/archive/v0.5.0.tar.gz
    $ tar -xzvf ruby-install-0.5.0.tar.gz
    $ cd ruby-install-0.5.0/
    $ sudo make install


You now have the ruby-install command at your disposal.

To install a ruby by version is easy:

`$ ruby-install ruby 2.2.2`

At this point you should see alot of stdout in your terminal window.

It will go through a process of installing the source and compiling your ruby into your ~/.rubies folder.

Chruby will know to automatically look in both ~/.rubies and /opt/rubies for rubies installed.

After the install is completed, source your shell rc file.
`$ source ~/.bashrc`

Now execute chruby to select the newly installed ruby:

`$ chruby`

You should see something like this:

    ubuntu@nzb:~$ chruby
       ruby-2.1.3
       ruby-2.2.2
   
Now select the ruby you want to use:
    
    ubuntu@nzb:~$ chruby 2.2.2
    ubuntu@nzb:~$ chruby
       ruby-2.1.3
     * ruby-2.2.2

Boom. Ruby 2.2.2 is ready to rock.

    ubuntu@nzb:~$ ruby -v
    ruby 2.2.2p95 (2015-04-13 revision 50295) [x86_64-linux]

To take advantage of the auto-switching capabilities simply add a `.ruby-version` file to the directory specifying a specific version you need ruby to be. I usually set a default version in my home directory.

`echo 'ruby-2.2.2' > ~/.ruby-version`

Now you should have ruby 2.2.2 ready to go whenever you login.

