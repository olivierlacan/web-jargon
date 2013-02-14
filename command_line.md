# The Command Line

This is often reffered to as "The Shell" or more commonly as a "promt" or "terminal" because neckbeards can't be bothered to be consistent with anything.

## $PATH

This is the load path of your command line utility. The application used to access the command line on OS X is called Terminal, but the underlying utility that actually powers this command line is called __Bash__. 

Some adventurous neckbeards don't like the relative simplicity of this utility and often run in circles to find something more obscure and ""powerful"" to use, like ZSH, which is such an absolutely unpronounceable acronym that everybody calls it "Z Shell". Why they didn't decide to simply fucking name it Z Shell to begin with is beyond me, and quite worthy of this digression.

Now back to the path.

See what I did there?

To understand what the $PATH is, you must first understand why the hell there is a dollar sign in front of it. In the command line, as in most programming languags, when something is writtten in all-caps it usually means that it's a [_constant_](programming.md#constant). The dollar sign is often used to denote [_variables_](programming.md#variable) as well.

In this case, $PATH is an [environment variable](), something that will be used across the entire environment — your command line utility.

A command line can do two simple things:
- run code
- execute programs

The $PATH comes in handy for that second part. You see, the operating system (in this scenario, OS X) comes with many different pre-installed applications. So does the command line. In the command line, instead of referring to programs as applications, they're simply called [binaries]() because they're compiled binary representations of an original source code.

Unlike the web, most operating systems use compiled languages that can't simply execute a program on the fly, they first need to be compiled into machine-readable code that the computer can understand. That compilation process also customizes the code depending on the operating system it's dealing with.

Anyway, the load path — our $PATH — is simply a list of places where the command line utility will look for programs to execute.

To see what's in your $PATH you can run the following line of code:

```bash
echo $PATH
```

And you should see something similar to this: 

`/usr/local/heroku/bin:./bin:/Users/olivierlacan/.rbenv/shims:/usr/local/bin:/usr/local/sbin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin`

Although I do wish yours is a bit less verbose than mine.

As you can see all those words are separated by two things: either slashes, or colons. The colons separate each load path value, while the slashes are simply how Unix systems denote different directories (or folders).

Take the first one: `/usr/local/heroku/bin`

- the first slash means we're starting at the __root__ of the system.
- `usr/` is the location of the `usr` directory, directly under the root.
- `local` is a sub-directory located under `usr`
- `heroku` is another sub-directory under `local`
- and finally `bin` is a directory where __binaries__ are stored

All this is is an absolute path to the `bin` directory, from the very root of the system to where it is.

If we look inside of that path, we can find one binary waiting to be executed:

```bash
$ ls /usr/local/heroku/bin
heroku
```

If I type the entire path including the binary name, the executable binary is called:

```bash
 /usr/local/heroku/bin/heroku 
Usage: heroku COMMAND [--app APP] [command-specific-options]

Primary help topics, type "heroku help TOPIC" for more details:

  addons    #  manage addon resources
  apps      #  manage apps (create, destroy)
  auth      #  authentication (login, logout)
  config    #  manage app config vars
  domains   #  manage custom domains
  logs      #  display logs for an app
  ps        #  manage processes (dynos, workers)
  releases  #  manage app releases
  run       #  run one-off commands (console, rake)
  sharing   #  manage collaborators on an app

Additional topics:

  account      #  manage heroku account options
  certs        #  manage ssl endpoints for an app
  db           #  manage the database for an app
  drains       #  display syslog drains for an app
  git          #  manage git for apps
  help         #  list commands and display help
  keys         #  manage authentication keys
  labs         #  manage optional features
  maintenance  #  manage maintenance mode for an app
  pg           #  manage heroku-postgresql databases
  pgbackups    #  manage backups of heroku postgresql databases
  plugins      #  manage plugins to the heroku gem
  ssl          #  manage ssl certificates for an app
  stack        #  manage the stack for an app
  status       #  check status of heroku platform
  update       #  update the heroku client
  version      #  display version
```

As is convention for most Unix binaries, the heroku binary will tell me what commands it offers when I call it without any _arguments_.

An example argument would be "version", which apparently should tell me which version of the heroku binary I'm using:

```bash
$ /usr/local/heroku/bin/heroku version
heroku-toolbelt/2.35.0 (x86_64-darwin10.8.0) ruby/1.9.3
```

- this is version 2.35.0. 
- it was designed for x86 infrastructures, which most modern operating system for desktop computers use
- it was designed for 64 bit systems
- [darwin](http://en.wikipedia.org/wiki/Darwin_(operating_system)) 10.8.0 is the name and version number of the underlying operating system which powers OS X and iOS.
- finally ruby/1.9.3 is the version of Ruby for which this heroku binary was designed

Now let's get back to our multiple paths from before and split them line by line, which the operating system does for us anyway:

```bash
/usr/local/heroku/bin
./bin
/Users/olivierlacan/.rbenv/shims
/usr/local/bin
/usr/local/sbin
/usr/bin
/bin
/usr/sbin
/sbin
/usr/local/bin
/opt/X11/bin
```

This $PATH is in order of priority from left to right, or top to bottom here. First the system will look inside of the first directory when you try to call any binary. Let's say we're looking for `ruby`. OS X comes with a pre-installed version of Ruby, these days it's version 1.8.7.

That binary is stored inside of `/usr/bin`, so if we ran:

```bash
$ /usr/bin/ruby --version
```
We would see displayed:

`ruby 1.8.7 (2012-02-08 patchlevel 358) [universal-darwin12.0]`

Except there is another directory higher in the $PATH called `/usr/local/bin`. As its name suggests, it's intended for local binaries, instead of system ones. It's used by a great OS X package manager called [Homebrew](http://mxcl.github.com/homebrew/) to install binaries if you'd rather not use (or update) the pre-installed ones OS X ships with.

I once installed a version of Ruby using Homebrew, which means there should be a ruby binary sitting in the directory. Let's check.

```bash
$ /usr/local/bin/ruby --version
ruby 1.9.3p362 (2012-12-25 revision 38607) [x86_64-darwin12.2.1]
```

Well, that's clearly a different version of Ruby right there. It's actually the most recent one since I used Homebrew to install it.

So wait a minute, there are two different versions of ruby available on my system. How does the system figure out which to use?

It goes the lazy way, by simply using the first one it can find. In this situation, the system would look inside of `/usr/local/bin` first and therefore use this ruby binary over the other one. But so far we've been using the absolute path to call the ruby executable, so let's see the system do its thing:

```bash
ruby --version
ruby 1.9.3p362 (2012-12-25 revision 38607) [x86_64-darwin12.2.1]
```

We can tell it's that same ruby version again, but how can we be sure it's the one that came from `/usr/local/bin`? We can ask:

```bash
which ruby
/usr/local/bin/ruby
```

And that, my deliciously gender neutral moniker, is how the load $PATH works.
