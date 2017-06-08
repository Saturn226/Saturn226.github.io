---
layout: post
title:  "Restarting a stopped job with FG"
date:   2017-06-08 16:35:28 +0000
---


Are you like me, where you can't remember the given shortcut for anything at any given time?
Do you have a tendency to press Ctrl + Z to stop the rails server and then jackup your port?

There is an easy fix for that and that fix is `fg` . By typing `fg` in your terminal, the last stopped job will resume in the foreground

```
// ♥ rails s
=> Booting WEBrick
=> Rails 4.2.6 application starting in development on http://localhost:3000
=> Run `rails server -h` for more startup options
=> Ctrl-C to shutdown server
[2017-06-08 12:25:36] INFO  WEBrick 1.3.1
[2017-06-08 12:25:36] INFO  ruby 2.3.0 (2015-12-25) [x86_64-darwin16]
[2017-06-08 12:25:36] INFO  WEBrick::HTTPServer#start: pid=19135 port=3000
^Z
[1]+  Stopped                 rails s
```

This is what it looks like when you stop a job with Ctrl+Z

Now for `fg`

```
// ♥ fg
rails s
```

And when I navigate back to my home page, we can ensure that the server is in fact running again

```
Started GET "/" for ::1 at 2017-06-08 12:26:48 -0400
  ActiveRecord::SchemaMigration Load (1.0ms)  SELECT "schema_migrations".* FROM "schema_migrations"
Processing by ApplicationController#index as HTML
  Rendered application/index.html.erb within layouts/application (3.0ms)
Completed 200 OK in 1234ms (Views: 1224.2ms | ActiveRecord: 0.0ms)

```

Now we can close the server down properly with Ctrl+ C

```
^C[2017-06-08 12:27:30] INFO  going to shutdown ...
[2017-06-08 12:27:30] INFO  WEBrick::HTTPServer#start done.
Exiting
```




