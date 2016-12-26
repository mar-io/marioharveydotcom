+++
date = "2015-05-09T00:09:30-05:00"
title = "SVN and Keepalive"
draft = false

+++

![cover](/content/images/2015/Mar/stayinalive.jpg)

If you find yourself in the unfortunate position of being responsible for a project using svn (==move to [Git](http://git-scm.com/) already it's **2015**==), you may want to make sure to enable the Apache [KeepAlive directive](http://httpd.apache.org/docs/2.2/mod/core.html#keepalive) in the httpd.conf on the svn server.

This setting will allow multiple requests (files, text, etc.) to come over the same tcp connection. This is awesome in the case of large svn repos because if that connection breaks you will often get an error like:

**`svn: E175002: Connection reset`**

This will cause your checkout to fail and you will have to start the whole thing again and it will probably fail again with the same error.

So enable keep alive by finding the http.conf on your svn server usually located at `/etc/httpd/conf/httpd.conf`.

Open the file and make the following changes:

    # KeepAlive: Whether or not to allow persistent connections (more than
    # one request per connection). Set to "Off" to deactivate.
    #
    KeepAlive On
    
    #
    # MaxKeepAliveRequests: The maximum number of requests to allow
    # during a persistent connection. Set to 0 to allow an unlimited amount.
    # We recommend you leave this number high, for maximum performance.
    #
    MaxKeepAliveRequests  200 (this can change based on how many users at peak usage)
    #
    # KeepAliveTimeout: Number of seconds to wait for the next request from the
    # same client on the same connection.
    #
    KeepAliveTimeout 3

The benefits of using `KeepAlive` is an **increase in the speed and reliability of svn while reducing cpu usage** since it will not open and close as many connections when checking out code.

The downside is that **memory usage will increase** due to the fact that svn will be holding many connections at same time and waiting for requests and responses over those connections. So make sure to monitor memory usage and gauge what is right for you.

In conclusion, switch to Git.