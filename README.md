# Using pyenv to install python libraries for Nuke

## pyenv
Pyenv allows multiple versions of python to be installed on your machine and easily switch between those versions.

Before continuing the tutorial, you'll need to install pyenv and pyenv-virtualenv.

Find out how to to install pyenv for your operating system here:\
https://github.com/pyenv/pyenv

Installing pyenv-virtualenv will allow us to isolate our installation in it's own environment:\
https://github.com/pyenv/pyenv-virtualenv

Now that pyenv and pyenv-virtualenv are installed, let's take a look at Nuke.

## What version of python does Nuke use?
In order to make sure we don't have version conflicts with our installed python libraries, we need to make sure we're using the same version of python as Nuke. 

What version does Nuke use? Let's find out!

In Nuke, open your script editor and let's use the [platform](https://docs.python.org/2.7/library/platform.html) library
to find out more about Nuke's python version.
```python
import platform
print platform.python_version()
```

You can find out even more detailed information by using sys
```python
import sys
print sys.version
```

I'm using Nuke 12.1v3 for this tutorial so the result for either option is 2.7.16.

Now that we know the version that Nuke uses, let's install it using pyenv

## Install Nuke's version of python
Use pyenv's install command to install 2.7.16
```commandline
pyenv install 2.7.16
```

Make a virtual instance for our nuke installs
```commandline
pyenv virtualenv 2.7.16 nuke12
```

Let's run the same commands we ran in Nuke to find out more about our python install.
Let's activate our new instance on 2.7.16.
```commandline
pyenv shell nuke12
```

```commandline
python -c "import platform; print platform.python_version()"
```

```commandline
python -c "import sys; print sys.version"
```

Install some python libraries
```commandline
pip install pyyaml
pip install pymongo
```

We need to find the path to where the nuke12 virtualenv places it's site-packages.
We'll use this to link it in our init.py file
```commandline
python -c 'import site; print(site.getsitepackages())'
# '/Users/homedir/.pyenv/versions/nuke12/lib/python2.7/site-packages'
```

Using sys, we can append the path that python uses to source imports.\
Put this in your init.py file
```python
import sys
sys.path.append("/Users/homedir/.pyenv/versions/nuke12/lib/python2.7/site-packages")
```

Open up Nuke and in the script editor, try an import of one of the two libraries we installed.
```python
try:
    import yaml
except Exception as e:
    print str(e)

try:
    import pymongo
except Exception as e:
    print str(e)
```

If you don't get any errors, that's it.\
Happy Comping!
