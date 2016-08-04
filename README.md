# How to Install a Local Python Version on RHEL Servers

Because the base Python for a given system is likely to be a dependent for a
wide variety of tools (e.g. yum in Redhat Enterprise is written in Python),
there may arise a need to install and use a version of Python that is newer
than the existing system version. This tutorial runs through how to do that.

In this folder, you will find the most recent (as of 8/4/2016) Python
distribution and virtualenv distribution. I encourage you to [check if newer
versions are available](https://www.python.org/downloads/) and to download their tar/gz files to use instead.

Once installed, we need a place to put the files. Let's call it `src`.

`mkdir ~/src/`  
`cd src`


Put the zipped distributions into this folder.

### Installing Python

First, unzip the files and move into the new directory created by that process.

`tar -zxvf Python-2.7.12.tgz`  
`cd Python-2.7.12`  

We'll then create a hidden directory which will hold both Python and
virtualenv.

`mkdir ~/.localpython`

We then install the Python distribution into that folder.

`./configure --prefix=$HOME/.localpython`  
`make`  
`make install`

Congrats! You just installed the latest version of Python for your user.

### Installing virtualenv

Problematically, you can't just start downloading modules into your new Python
version -- the pip accessed from the commandline will only install to the base
Python version for the whole system. Instead, you need to install a virtual
environment which will produce an instance wherein those packages can be
installed for your local user.

The process is similar. First, we unzip then `cd` into the new folder.

`tar -zxvf virtualenv-15.0.2.tar.gz`  
`cd virtualenv-15.0.2/`

We then run the setup script specifying the new Python we just installed:

`~/.localpython/bin/python setup.py install`  

You may need to run that last line twice. To double-check if you do, check if an
executable called `virtualenv` is in the `~/.localpython/bin` directory. Use the
following command; it should look like this:

`ls ~/.localpython/bin`  
> `2to3  pydoc   python2    python2.7-config  python-config virtualenv idle  
python  python2.7  python2-config    smtpd.py`  

Once that executable is in there, think of what you want to name your
virtualenv. Then, run the following code:

`~/.localpython/bin/virtualenv ##THE NAME YOU WANT## -p $HOME/.localpython/bin/python2.7`

Now it's installed! To run, you can use:

`source ##THE NAME YOU CHOSE##/bin/activate`

### Create a symbolic link to your virtualenv

Let's make it pretty so you don't have to write all that. `cd` to wherever you'd
like to keep the activation link. I just went to my local root:

`cd ~`

And use this code, which basically creates a shortcut:

`ln -s ~/src/virtualenv-15.0.2/##THE NAME YOU CHOSE##/bin/activate activate`

Now, to start your session whenever you're logged into the server, just navigate
to this directory and type:

`source activate`

Happy coding!
