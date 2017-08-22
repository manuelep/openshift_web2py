What's it?
===

This repository comes from a fork of the original project
"[openshift_web2py](https://github.com/prelegalwonder/openshift_web2py)" with the
same purposes of helping you get up and running quickly with a web2py
installation on [OpenShift](https://www.openshift.com/) service
[version 2](https://openshift.redhat.com/app/login?then=%2Fapp%2Fconsole).

# How to use it



# How I did it

![how I did it](https://nevalalee.files.wordpress.com/2012/04/how-i-did-it.jpg "how I did it")

## The step by step procedure

### Clone the **openshift_web2py** integration project repository

[openshift_web2py](https://github.com/prelegalwonder/openshift_web2py) is a
repository that helps you get up and running quickly with a web2py installation on
OpenShift.

```bash
git clone https://github.com/prelegalwonder/openshift_web2py.git
cd openshift_web2py
```

#### Modify the **openshift_web2py** integration project repository

In order to mantein the project synched with the framework evolution link up
the web2py project as a [submodule](https://git-scm.com/docs/git-submodule).

1. Fix the environment library import reference setting

    Change the line 10 in `wsgi/application` from

    ```python
    sys.path.append(os.path.join(os.environ['OPENSHIFT_REPO_DIR'], 'libs', 'gluon'))
    ```

    to

    ```python
    sys.path.append(os.path.join(os.environ['OPENSHIFT_REPO_DIR'], 'wsgi', 'web2py', 'gluon'))
    ```

1. Fix the web2py log reference setting

    Change line 13 in `wsgi/application` from

    ```python
    WEB2PY_LOG = os.path.join(os.environ['OPENSHIFT_PYTHON_LOG_DIR'],'web2py.log')
    ```

    to

    ```python
    WEB2PY_LOG = os.path.join(os.environ['OPENSHIFT_LOG_DIR'],'web2py.log')
    ```

1. Remove outdated web2py code

    ```bash
    git rm -rf wsgi/web2py
    git rm -rf libs/gluon
    ```

1. Add the framework project as a new submodule

    ```bash
    git submodule add https://github.com/web2py/web2py.git wsgi/web2py
    ```

1. Fix the framework version

    ```bash
    cd wsgi/web2py
    git checkout tags/R-2.14.6
    cd ../../
    git submodule update --init --recursive
    ```

1. Fix all changes made

    ```bash
    git add .
    git commit -am "linked web2py as submodule and fixed version to r-2.14.6"
    git tag -a web2py-R.2.14.6 -m "Openshift Web2py R-2.14.6"
    git push
    git git push origin web2py-R.2.14.6
    ```
